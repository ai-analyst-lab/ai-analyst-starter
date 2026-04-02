# Schema: NovaMart

## users
_Customer dimension table with demographics and acquisition info._
**Rows:** 50,000

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| `user_id` | INTEGER | No | Primary key |
| `signup_date` | DATE | No | Date user registered |
| `signup_timestamp` | TIMESTAMP | No | Exact registration time |
| `acquisition_channel` | VARCHAR | No | How user was acquired (organic, paid, referral, etc.) |
| `country` | VARCHAR | No | User country |
| `device_primary` | VARCHAR | No | Primary device type |
| `age_bucket` | VARCHAR | No | Age range bucket |
| `gender` | VARCHAR | No | User gender |

## orders
_Transaction records with amounts, status, and promo info._
**Rows:** 47,199

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| `order_id` | INTEGER | No | Primary key |
| `user_id` | INTEGER | No | FK to users |
| `order_timestamp` | TIMESTAMP | No | Exact order time |
| `order_date` | DATE | No | Order date |
| `subtotal` | FLOAT | No | Pre-discount amount |
| `discount_amount` | FLOAT | No | Discount applied |
| `shipping_amount` | FLOAT | No | Shipping cost |
| `total_amount` | FLOAT | No | Final amount charged |
| `status` | VARCHAR | No | Order status (completed, cancelled, etc.) |
| `promo_id` | FLOAT | Yes | FK to promotions (null if no promo) |
| `is_plus_member_order` | BOOLEAN | No | Whether placed by Plus member |
| `device` | VARCHAR | No | Device used for order |
| `session_id` | VARCHAR | No | FK to sessions |

## order_items
_Line items within each order._
**Rows:** 75,447

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| `order_item_id` | INTEGER | No | Primary key |
| `order_id` | INTEGER | No | FK to orders |
| `product_id` | INTEGER | No | FK to products |
| `quantity` | INTEGER | No | Units purchased |
| `unit_price` | FLOAT | No | Price per unit |
| `discount_amount` | FLOAT | No | Line-level discount |
| `line_total` | FLOAT | No | Final line amount |

## products
_Product catalog with category hierarchy and pricing._
**Rows:** 500

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| `product_id` | INTEGER | No | Primary key |
| `product_name` | VARCHAR | No | Product display name |
| `category` | VARCHAR | No | Top-level category |
| `subcategory` | VARCHAR | No | Sub-category |
| `price` | FLOAT | No | Retail price |
| `cost` | FLOAT | No | Product cost |
| `is_plus_eligible` | BOOLEAN | No | Whether eligible for Plus member benefits |

## sessions
_Aggregated session-level metrics from clickstream._
**Rows:** 1,383,467

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| `session_id` | VARCHAR | No | Primary key |
| `user_id` | INTEGER | No | FK to users |
| `session_start` | TIMESTAMP | No | Session start time |
| `session_end` | TIMESTAMP | No | Session end time |
| `session_date` | DATE | No | Session date |
| `device` | VARCHAR | No | Device type |
| `landing_page` | VARCHAR | No | First page viewed |
| `page_views` | INTEGER | No | Pages viewed in session |
| `events_count` | INTEGER | No | Total events in session |
| `had_purchase` | BOOLEAN | No | Whether session included a purchase |

## events
_Raw clickstream events (page views, clicks, searches, purchases)._
**Rows:** 6,510,093

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| `event_id` | VARCHAR | No | Primary key |
| `user_id` | INTEGER | No | FK to users |
| `session_id` | VARCHAR | No | FK to sessions |
| `event_timestamp` | TIMESTAMP | No | Event time |
| `event_date` | DATE | No | Event date |
| `event_type` | VARCHAR | No | Event category (page_view, add_to_cart, purchase, search, etc.) |
| `device` | VARCHAR | No | Device type |
| `product_id` | FLOAT | Yes | FK to products (null for non-product events) |
| `page_url` | VARCHAR | Yes | Page URL |
| `search_query` | VARCHAR | Yes | Search term (null for non-search events) |
| `app_version` | VARCHAR | No | Application version |

## memberships
_Plus membership program records._
**Rows:** 5,513

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| `membership_id` | INTEGER | No | Primary key |
| `user_id` | INTEGER | No | FK to users |
| `plan_type` | VARCHAR | No | Membership tier |
| `started_at` | TIMESTAMP | No | Membership start |
| `ended_at` | TIMESTAMP | Yes | Membership end (null if active) |
| `status` | VARCHAR | No | active, cancelled, expired |
| `cancel_reason` | VARCHAR | Yes | Reason for cancellation |
| `is_current` | BOOLEAN | No | Whether currently active |

## experiment_assignments
_A/B test variant assignments._
**Rows:** 20,000

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| `assignment_id` | INTEGER | No | Primary key |
| `experiment_id` | INTEGER | No | FK to experiments |
| `user_id` | INTEGER | No | FK to users |
| `variant` | VARCHAR | No | control or treatment |
| `assigned_date` | DATE | No | Date assigned |
| `first_exposure_date` | DATE | No | First exposure to variant |

## experiments
_Experiment definitions and metadata._
**Rows:** 2

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| `experiment_id` | INTEGER | No | Primary key |
| `experiment_name` | VARCHAR | No | Human-readable name |
| `hypothesis` | VARCHAR | No | Test hypothesis |
| `primary_metric` | VARCHAR | No | Primary success metric |
| `guardrail_metrics` | VARCHAR | No | Guardrail metrics (comma-separated) |
| `start_date` | DATE | No | Experiment start |
| `end_date` | DATE | No | Experiment end |
| `status` | VARCHAR | No | running, completed, stopped |

## nps_responses
_Net Promoter Score survey responses._
**Rows:** 8,000

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| `response_id` | INTEGER | No | Primary key |
| `user_id` | INTEGER | No | FK to users |
| `response_date` | DATE | No | Survey response date |
| `score` | INTEGER | No | NPS score (0-10) |
| `user_segment` | VARCHAR | No | User segment at time of response |
| `device` | VARCHAR | No | Device used |
| `comment` | VARCHAR | Yes | Free-text feedback |

## support_tickets
_Customer support interactions._
**Rows:** 21,587

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| `ticket_id` | INTEGER | No | Primary key |
| `user_id` | INTEGER | No | FK to users |
| `created_at` | TIMESTAMP | No | Ticket creation time |
| `created_date` | DATE | No | Ticket creation date |
| `category` | VARCHAR | No | Issue category |
| `severity` | VARCHAR | No | Severity level |
| `status` | VARCHAR | No | open, resolved, escalated |
| `resolved_at` | TIMESTAMP | Yes | Resolution time (null if open) |
| `device` | VARCHAR | No | Device |
| `app_version` | VARCHAR | No | App version |
| `order_id` | FLOAT | Yes | Related order (null if not order-related) |

## promotions
_Promotional campaigns._
**Rows:** 5

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| `promo_id` | INTEGER | No | Primary key |
| `promo_name` | VARCHAR | No | Campaign name |
| `promo_type` | VARCHAR | No | Type of promotion |
| `discount_pct` | FLOAT | No | Discount percentage |
| `start_date` | DATE | No | Campaign start |
| `end_date` | DATE | No | Campaign end |
| `target_segment` | VARCHAR | No | Targeted user segment |

## calendar
_Date dimension table._
**Rows:** 366

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| `date` | DATE | No | Calendar date |
| `day_of_week` | VARCHAR | No | Day name |
| `is_weekend` | BOOLEAN | No | Weekend flag |
| `month` | INTEGER | No | Month number |
| `quarter` | INTEGER | No | Quarter (1-4) |
| `is_holiday` | BOOLEAN | No | Holiday flag |
| `holiday_name` | VARCHAR | Yes | Holiday name (null if not holiday) |
