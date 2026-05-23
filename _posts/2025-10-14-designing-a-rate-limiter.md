---
layout: post
title: "Designing a Rate Limiter"
date: 2025-10-14
category: "System Design"
excerpt: "Token bucket, sliding window, and distributed rate limiting strategies for protecting API services from abuse."
---

Rate limiting sits at the intersection of reliability engineering and product management. Too strict and you frustrate legitimate users; too lenient and a single misbehaving client can degrade service for everyone.
