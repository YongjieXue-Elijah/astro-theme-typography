---
title: Chapter17.
pubDate: 2025-04-20
categories: ['SystemDesign']
description: ''
slug: systemdesign
---

# Proximity Service

A proximity service enables you to discover nearby places such as restaurants, hotels, theatres, etc.

# Step 1 - Understand the problem and establish design scope
Sample questions to understand the problem better:
 * C: Can a user specify a search radius? What if there are not enough businesses within the search area?
 * I: We only care about businesses within a certain area. If time permits, we can discuss enhancing the functionality.
 * C: What's the max radius allowed? Can I assume it's 20km?
 * I: Yes, that is a reasonable assumption
 * C: Can a user change the search radius via the UI?
 * I: Yes, let's say we have the options - 0.5km, 1km, 2km, 5km, 20km
 * C: How is business information modified? Do we need to reflect changes in real-time?
 * I: Business owners can add/delete/update a business. Assume changes are going to be propagated on the next day.
 * C: How do we handle search results while the user is moving?
 * I: Let's assume we don't need to constantly update the page since users are moving slowly.

## Functional requirements
 * Return all businesses based on user's location
 * Business owners can add/delete/update a business. Information is not reflected in real-time.
 * Customers can view detailed information about a business

## Non-functional requirements
 * Low latency - users should be able to see nearby businesses quickly
 * Data privacy - Location info is sensitive data and we should take this into consideration in order to comply with regulations
 * High availability and scalability requirements - We should ensure system can handle spike in traffic during peak hours in densely populated areas

## Back-of-the-envelope calculation
 * Assuming 100mil daily active users and 200mil businesses
 * Search QPS == 100mil * 5 (average searches per day) / 10^5 (seconds in day) == 5000

# Step 2 - Propose High-Level Design and get Buy-In
## API Design
We'll use a RESTful API convention to design a simplified version of the APIs.
```
GET /v1/search/nearby
```

This endpoint returns businesses based on search criteria, paginated.

Request parameters - latitude, longitude, radius

Example response:
```
{
  "total": 10,
  "businesses":[{business object}]
}
```

The endpoint returns everything required to render a search results page, but a user might require additional details about a particular business, fetched via other endpoints.

Here's some other business APIs we'll need:
 * `GET /v1/businesses/{:id}` - return business detailed info
 * `POST /v1/businesses` - create a new business
 * `PUT /v1/businesses/{:id}` - update business details
 * `DELETE /v1/businesses/{:id}` - delete a business

Example pseudocode to build a quadtree:
```
public void buildQuadtree(TreeNode node) {
    if (countNumberOfBusinessesInCurrentGrid(node) > 100) {
        node.subdivide();
        for (TreeNode child : node.getChildren()) {
            buildQuadtree(child);
        }
    }
}
```

In a leaf node, we store:
 * Top-left, bottom-right coordinates to identify the quadrant dimensions
 * List of business IDs in the grid

In an internalt node we store:
 * Top-left, bottom-right coordinates of quadrant dimensions
 * 4 pointers to children

The total memory to represent the quadtree is calculated as ~1.7GB in the book if we assume that we operate with 200mil businesses.

Hence, a quadtree can be stored in a single server, in-memory, although we can of course replicate it for redundancy and load balancing purposes.

One consideration to take into consideration if this approach is adopted - startup time of server can be a couple of minutes while the quadtree is being built.

Hence, this should be taken into account during the deployment process. Eg a healthcheck endpoint can be exposed and queried to signal when the quadtree build is finished.

Another consideration is how to update the quadtree. Given our requirements, a good option would be to update it every night using a nightly job due to our commitment of reflecting changes at start of next day.

It is nevertheless possible to update the quadtree on the fly, but that would complicate the implementation significantly.

# Step 4 - Wrap Up
Summary of some of the more interesting topics we covered:
 * Discussed several indexing options - 2d search, evenly divided grid, geohash, quadtree, google S2
 * Discussed caching, replication, sharding, cross-DC deployments in the deep dive section
