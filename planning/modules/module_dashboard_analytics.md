# Dashboard Analytics Module

## Overview
The Dashboard Analytics module provides a high-level visual overview of the system's key performance indicators (KPIs). It translates raw data from various modules (TNA, Users, etc.) into interactive charts and summaries to help stakeholders make informed decisions.

## Actors
- **Learning Coordinator**: Views analytics related to TNA submissions and training trends.
- **Learning Administrator**: Views overall platform activity and user statistics.

## Features
- **Submission Trend (Area Chart)**: Visualizes the volume of TNA submissions over a 6-month period using interactive smooth area charts.
- **Status Distribution (Donut Chart)**: Shows the real-time breakdown of submissions by status (Approved, Review, Draft, Rejected).
- **Interactive Metrics**: Clickable summary cards that show totals for each status category.
- **Responsive Visualization**: Charts that automatically resize based on the layout and sidebar state.

## Technical Implementation
- **Library**: ApexCharts (CDN integrated into the master layout).
- **Responsiveness**: Enforced via `width: 100%` and `setTimeout` delayed rendering to wait for sidebar transitions.
- **Data Syncing**: Powered by backend statistical aggregation in the Dashboard Controller.

## Laravel Implementation Mapping

### Controllers
- `DashboardController` (Aggregates stats and prepares data for charts)

### Views
- `pages/lc/index.blade.php` (Main dashboard implementation with ApexCharts JS)
- `layouts/backoffice.blade.php` (Library integration)

## Dependencies
depends_on:
- Training Management Module
- User & Role Management
used_by: []
