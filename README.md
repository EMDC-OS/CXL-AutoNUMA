# Optimizing Page Migration for Hierarchical Memory Systems

## Overview
In hierarchical memory systems, Linux kernel's AutoNUMA memory tiering mode dynamically migrates frequently accessed hot pages to higher-tier memory (e.g., DRAM) while demoting less frequently accessed pages to lower-tier memory (e.g., CXL-attached memory). This approach aims to optimize memory placement based on temporal locality.

However, our research has identified that the dynamic threshold adjustment method in recent kernels leads to excessive page migrations, causing performance degradation due to additional overhead from page table scans, locking, and page copying. Additionally, frequent page migrations between tiers result in a "ping-pong" effect, further impacting performance.

## Problem with Current AutoNUMA Implementation
The existing Linux kernel limits excessive page promotion with a predefined threshold of 64 GB/s. However, our analysis has shown that this threshold is too high, causing the migration threshold to rise excessively, resulting in unnecessary page promotions.

## Proposed Optimization
To address this issue, we propose a modified AutoNUMA implementation in Linux kernel 6.8 that allows users to set a configurable "migration threshold" dynamically at runtime. Our approach adjusts the hot page migration threshold based on user-defined values rather than relying on the static 64 GB/s limit.

Key enhancements include:
1. **Configurable Migration Threshold:** Users can define the threshold for hot page promotion.
2. **Reduced Overhead from Excessive Migration:** More careful promotion strategy to minimize unnecessary memory movement.
3. **Preserving Kernel Intentions:** Our method still respects the kernel’s original objective of limiting excessive page promotion while refining the approach for better efficiency.

## Conclusion
Our optimized page migration strategy enables more precise promotion of hot pages, reducing unnecessary memory movements and improving overall system efficiency. This modification allows greater control over memory tiering behavior, making it better suited for hierarchical memory environments.

---


