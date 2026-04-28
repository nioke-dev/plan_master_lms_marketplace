# Training Management Module

## Overview
This module facilitates the end-to-end lifecycle of enterprise training programs, starting from the identification of training needs (TNA) to the final approval and scheduling of training programs. It ensures that all training activities are aligned with organizational goals and employee competency requirements.

## Actors
- **Learning Coordinator**: Responsible for identifying training needs, submitting TNA requests, and monitoring the status of submissions.
- **Admin Coordinator**: Reviews and approves/rejects TNA submissions.
- **SME (Subject Matter Expert)**: Provides input on technical training content.

## Features
- **TNA Submission**: Allows Learning Coordinators to propose new training programs.
- **Interactive Submission List**: A responsive, filterable list of all TNA requests with real-time status updates.
- **Advanced Filtering**: Search by ID/Name, Category, and Date Range (Litepicker integration).
- **Action Protection**: Logic to disable edit/delete actions for Approved or Rejected submissions to maintain data integrity.
- **Export Reports**: Synchronized Excel export functionality based on active filters.

## Functional Requirements
- System must allow filtering by date range using a calendar interface.
- System must provide horizontal scroll for the submission table on smaller screens.
- System must prevent modification of data once it reaches a final status (Approved/Rejected).
- System must return to page 1 of pagination whenever a filter is changed.

## Laravel Implementation Mapping

### Models
- `TnaSubmission` (Eloquent Model for TNA data)
- `TnaCategory` (Eloquent Model for training categories)

### Controllers
- `TnaSubmissionController` (Handles listing, creation, and export logic)

### Views
- `pages/lc/daftar-usulan.blade.php` (Main interactive list view)
- `components/tna/date-range-filter.blade.php` (Reusable calendar component)

## Dependencies
depends_on:
- Authentication
- User & Role Management
used_by:
- Learning Delivery
- Certification
- Survey & Evaluation
