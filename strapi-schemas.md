# Strapi Content Type Schemas for Travel Offers

## Overview

Based on the provided PDF offers (Bali, Beirut, Dahab, El Sokhna, Hurghada, Istanbul, Sahl Hasheesh, Sharm), this schema defines a simple structure for monthly hotel offers.

---

## Content Types

### 1. **Offer** (Collection Type)

#### Fields:

| Field Name    | Type                   | Required | Notes                             |
| ------------- | ---------------------- | -------- | --------------------------------- |
| title         | Text                   | Yes      | E.g., "Dahab - November Offer"    |
| slug          | UID                    | Yes      | Auto-generated from title         |
| destination   | Text                   | Yes      | E.g., "Dahab", "Istanbul", "Bali" |
| month         | Text                   | Yes      | E.g., "November", "Nov&Dec"       |
| year          | Number                 | No       | E.g., 2025                        |
| pdfFile       | Media (Single)         | Yes      | Original PDF file                 |
| coverImage    | Media (Single)         | No       | Main offer image                  |
| hotelOptions  | Component (repeatable) | Yes      | `offer.hotel-option`              |
| inclusions    | Component (repeatable) | No       | `offer.inclusion`                 |
| exclusions    | Component (repeatable) | No       | `offer.exclusion`                 |
| policies      | Rich Text              | No       | Kids policies, general policies   |
| optionalTrips | Component (repeatable) | No       | `offer.optional-trip`             |
| published     | Boolean                | No       | Default: false                    |

---

## Components

### **offer.hotel-option**

Individual hotel package with pricing.

#### Fields:

| Field Name     | Type    | Required | Notes                                            |
| -------------- | ------- | -------- | ------------------------------------------------ |
| hotelName      | Text    | Yes      | Name of hotel/resort                             |
| roomType       | Text    | No       | E.g., "Standard", "Double", "Sea View"           |
| nights         | Number  | Yes      | Number of nights (e.g., 3, 7)                    |
| mealPlan       | Text    | No       | E.g., "Breakfast", "Half Board", "All Inclusive" |
| pricePerPerson | Decimal | Yes      | Price per person                                 |
| pricePerChild  | Decimal | No       | Child price (if applicable)                      |
| currency       | Text    | Yes      | E.g., "USD", "EUR", "EGP"                        |
| notes          | Text    | No       | Additional details or conditions                 |

---

### **offer.inclusion**

Items included in the offer.

#### Fields:

| Field Name | Type | Required | Notes                                  |
| ---------- | ---- | -------- | -------------------------------------- |
| item       | Text | Yes      | E.g., "Airport transfers", "Breakfast" |

---

### **offer.exclusion**

Items NOT included in the offer.

#### Fields:

| Field Name | Type | Required | Notes                                  |
| ---------- | ---- | -------- | -------------------------------------- |
| item       | Text | Yes      | E.g., "Personal expenses", "Visa fees" |

---

### **offer.optional-trip**

Optional excursions/adventures available with the offer.

#### Fields:

| Field Name     | Type                   | Required | Notes                  |
| -------------- | ---------------------- | -------- | ---------------------- |
| title          | Text                   | Yes      | Trip/adventure name    |
| description    | Text                   | No       | Trip description       |
| pricePerPerson | Decimal                | Yes      | Price per person       |
| currency       | Text                   | Yes      | E.g., "EGP", "USD"     |
| inclusions     | Component (repeatable) | No       | `offer.trip-inclusion` |

---

### **offer.trip-inclusion**

Items included in an optional trip.

#### Fields:

| Field Name | Type | Required | Notes                                |
| ---------- | ---- | -------- | ------------------------------------ |
| item       | Text | Yes      | E.g., "Lunch", "Guide", "Entry fees" |

---

## Example Data Structure

### Example: Dahab November Offer

```json
{
  "title": "Dahab - November Offer",
  "slug": "dahab-november-offer",
  "destination": "Dahab",
  "month": "November",
  "year": 2024,
  "hotelOptions": [
    {
      "hotelName": "Swiss Inn Resort Dahab",
      "roomType": "Standard Room",
      "nights": 3,
      "mealPlan": "All Inclusive",
      "pricePerPerson": 150,
      "currency": "USD"
    },
    {
      "hotelName": "Tropitel Dahab Oasis",
      "roomType": "Sea View",
      "nights": 3,
      "mealPlan": "Half Board",
      "pricePerPerson": 180,
      "currency": "USD"
    }
  ],
  "inclusions": [
    { "item": "Airport transfers" },
    { "item": "Hotel accommodation" },
    { "item": "Meals as per plan" }
  ],
  "exclusions": [
    { "item": "Personal expenses" },
    { "item": "Travel insurance" },
    { "item": "Optional tours" }
  ],
  "policies": "**Kids Policy:**\n- Children under 6 years: Free\n- Children 6-12 years: 50% discount\n\n**General Policies:**\n- Cancellation 7 days before: Full refund\n- Cancellation 3-7 days: 50% refund\n- Less than 3 days: No refund",
  "optionalTrips": [
    {
      "title": "Blue Hole Snorkeling Trip",
      "description": "Discover the famous Blue Hole with professional guides",
      "pricePerPerson": 350,
      "currency": "EGP",
      "inclusions": [
        { "item": "Snorkeling equipment" },
        { "item": "Guide" },
        { "item": "Lunch" }
      ]
    },
    {
      "title": "Canyon Safari",
      "description": "Explore the colored canyon and Bedouin culture",
      "pricePerPerson": 500,
      "currency": "EGP",
      "inclusions": [
        { "item": "4x4 vehicle" },
        { "item": "Bedouin tea" },
        { "item": "Entry fees" }
      ]
    }
  ]
}
```
