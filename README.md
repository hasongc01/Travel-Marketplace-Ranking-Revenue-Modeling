# Travel-Marketplace-Ranking-Revenue-Modeling

# Summary
Developed predictive models using Expedia search session data to estimate click probability, post-click booking probability, and expected booking revenue for hotel listings. The project demonstrates how online travel marketplaces can rank properties not just by engagement (CTR) but by expected gross booking value, aligning search ranking with monetization outcomes.

# Business Context

Online travel marketplaces like Expedia must decide how to rank hotel listings when a user performs a search. A naïve approach ranks listings based on predicted click probability alone. However, a listing that receives many clicks may not necessarily lead to bookings or revenue.

The key business question is: How should Expedia rank properties in search results to maximize expected booking revenue while maintaining strong user engagement?

To answer this, the project builds models that estimate both user interest and booking likelihood, and combines them into a revenue-based ranking score.

# Data
Expedia search behavior dataset containing hotel impressions displayed during search sessions.

Dataset size
- ~200K+ search-result impressions
- Multiple properties displayed per search session (srch_id)

| Variable                  | Description                      |
| ------------------------- | -------------------------------- |
| `srch_id`                 | Search session identifier        |
| `position`                | Hotel position in search results |
| `price_usd`               | Displayed price                  |
| `promotion_flag`          | Whether property had promotion   |
| `prop_review_score`       | Customer review rating           |
| `visitor_hist_starrating` | Historical preferences of user   |
| `click_bool`              | Whether user clicked property    |
| `booking_bool`            | Whether user booked property     |
| `gross_booking_usd`       | Revenue generated from booking   |

# Methodology
## 1. Data Cleaning & Preparation

- Removed invalid observations (missing price data, negative durations, invalid records)
- Handled missing user history variables
- Normalized pricing and ranking variables
- Created session-level and property-level features

Example feature groups:
- Property features: star rating, review score, brand indicator
- Pricing signals: price, promotions, competitor price differences
- Search context: stay length, booking window, weekend indicator
- Ranking signals: search result position

## 2. Click-Through Rate (CTR) Model
Goal: estimate probability that a listing receives a click.

Approach:
- Logistic regression classifier
- XGB classifier 
- Features include position, price, promotions, property attributes

Purpose:
- Identify factors driving user engagement with search listings

## 3. Booking Conversion Model
Goal: estimate probability that a clicked listing converts to booking.

Approach:
- Logistic regression conditional on click events
- Focus on factors affecting purchase decision after interest

Purpose:
- Understand drivers of purchase behavior

## 4. Revenue-Based Ranking Score

To support marketplace ranking decisions, the models are combined into an expected booking value score:
- Expected Booking Revenue=P(click)×P(booking∣click)×Revenue 

Where revenue is proxied by: gross_booking_usd

This scoring model allows Expedia to rank properties by expected monetization potential, rather than engagement alone.

# Future Improvements

Apply learning-to-rank algorithms (LambdaMART, XGBoost ranking)

Incorporate session-level personalization

Optimize ranking via expected long-term customer value
