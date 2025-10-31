# Strapi Content Type Schemas for Travel Offers

## Overview

Based on the provided PDF offers (Bali, Beirut, Dahab, El Sokhna, Hurghada, Istanbul, Sahl Hasheesh, Sharm), this schema defines a simple structure for monthly hotel offers.

---

## Content Types

### 1. **Offer** (Collection Type)

#### Fields:

| Field Name   | Type                   | Required | Notes                             |
| ------------ | ---------------------- | -------- | --------------------------------- |
| title        | Text                   | Yes      | E.g., "Dahab - November Offer"    |
| slug         | UID                    | Yes      | Auto-generated from title         |
| destination  | Text                   | Yes      | E.g., "Dahab", "Istanbul", "Bali" |
| month        | Text                   | Yes      | E.g., "November", "Nov&Dec"       |
| year         | Number                 | No       | E.g., 2025                        |
| pdfFile      | Media (Single)         | Yes      | Original PDF file                 |
| coverImage   | Media (Single)         | No       | Main offer image                  |
| hotelOptions | Component (repeatable) | Yes      | `offer.hotel-option`              |
| published    | Boolean                | No       | Default: false                    |

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

Items/services included in the offer.

#### Fields:

| Field Name  | Type        | Required | Notes                                                              |
| ----------- | ----------- | -------- | ------------------------------------------------------------------ |
| title       | Text        | Yes      | E.g., "Airport Transfer"                                           |
| description | Text        | No       | Details about the inclusion                                        |
| icon        | Enumeration | No       | `plane`, `hotel`, `meal`, `transport`, `activity`, `wifi`, `other` |

---

### **offer.exclusion**

Items/services NOT included in the offer.

#### Fields:

| Field Name  | Type | Required | Notes                       |
| ----------- | ---- | -------- | --------------------------- |
| title       | Text | Yes      | E.g., "Personal Expenses"   |
| description | Text | No       | Details about the exclusion |

---

### **offer.itinerary-day**

Daily breakdown of the travel package.

#### Fields:

| Field Name    | Type                   | Required | Notes                     |
| ------------- | ---------------------- | -------- | ------------------------- |
| dayNumber     | Number                 | Yes      | Day sequence (1, 2, 3...) |
| title         | Text                   | Yes      | E.g., "Arrival in Bali"   |
| description   | Rich Text              | Yes      | Detailed day description  |
| meals         | Component (repeatable) | No       | `meal.meal-option`        |
| accommodation | Text                   | No       | Where staying this night  |
| activities    | Component (repeatable) | No       | `activity.activity-item`  |

---

### **accommodation.hotel-option**

Hotel/accommodation details.

#### Fields:

| Field Name    | Type             | Required       | Notes                                                                     |
| ------------- | ---------------- | -------------- | ------------------------------------------------------------------------- |
| hotelName     | Text             | Yes            | Name of hotel                                                             |
| hotelCategory | Enumeration      | Yes            | `3_star`, `4_star`, `5_star`, `resort`, `boutique`, `villa`               |
| location      | Text             | No             | Hotel location/area                                                       |
| roomType      | Text             | No             | E.g., "Deluxe Room", "Suite"                                              |
| nights        | Number           | Yes            | Number of nights                                                          |
| checkIn       | Date             | No             | Check-in date                                                             |
| checkOut      | No               | Check-out date |
| boardType     | Enumeration      | No             | `room_only`, `bed_breakfast`, `half_board`, `full_board`, `all_inclusive` |
| amenities     | JSON             | No             | Hotel amenities list                                                      |
| images        | Media (Multiple) | No             | Hotel images                                                              |

---

### **travel.flight-details**

Flight information component.

#### Fields:

| Field Name          | Type        | Required | Notes                                                   |
| ------------------- | ----------- | -------- | ------------------------------------------------------- |
| airline             | Text        | No       | Airline name                                            |
| flightNumber        | Text        | No       | Flight number                                           |
| departureAirport    | Text        | Yes      | E.g., "CAI"                                             |
| arrivalAirport      | Text        | Yes      | E.g., "DPS"                                             |
| departureDate       | Date        | No       | Departure date                                          |
| arrivalDate         | Date        | No       | Arrival date                                            |
| departureTime       | Time        | No       | Departure time                                          |
| arrivalTime         | Time        | No       | Arrival time                                            |
| cabinClass          | Enumeration | No       | `economy`, `premium_economy`, `business`, `first_class` |
| baggage             | Text        | No       | Baggage allowance                                       |
| hasReturnFlight     | Boolean     | No       | Default: false                                          |
| returnFlightDetails | Component   | No       | Nested `travel.flight-segment`                          |

---

### **travel.flight-segment**

Individual flight leg details.

#### Fields:

| Field Name       | Type | Required | Notes          |
| ---------------- | ---- | -------- | -------------- |
| airline          | Text | No       | Airline name   |
| flightNumber     | Text | No       | Flight number  |
| departureAirport | Text | Yes      | Airport code   |
| arrivalAirport   | Text | Yes      | Airport code   |
| departureDate    | Date | No       | Date           |
| departureTime    | Time | No       | Time           |
| arrivalTime      | Time | No       | Time           |
| duration         | Text | No       | E.g., "8h 30m" |

---

### **meal.meal-option**

Meal inclusions.

#### Fields:

| Field Name  | Type        | Required | Notes                                   |
| ----------- | ----------- | -------- | --------------------------------------- |
| mealType    | Enumeration | Yes      | `breakfast`, `lunch`, `dinner`, `snack` |
| description | Text        | No       | Meal details                            |
| location    | Text        | No       | Where meal is served                    |

---

### **activity.activity-item**

Activities included in the package.

#### Fields:

| Field Name     | Type    | Required | Notes                      |
| -------------- | ------- | -------- | -------------------------- |
| title          | Text    | Yes      | Activity name              |
| description    | Text    | No       | Activity details           |
| duration       | Text    | No       | E.g., "2 hours"            |
| included       | Boolean | Yes      | Is it included or optional |
| additionalCost | Decimal | No       | If optional, the cost      |

---

### **offer.terms-conditions**

Terms and conditions for the offer.

#### Fields:

| Field Name         | Type      | Required | Notes                      |
| ------------------ | --------- | -------- | -------------------------- |
| cancellationPolicy | Rich Text | No       | Cancellation terms         |
| paymentTerms       | Rich Text | No       | Payment requirements       |
| childPolicy        | Rich Text | No       | Children pricing/age rules |
| visaRequirements   | Rich Text | No       | Visa information           |
| healthRequirements | Rich Text | No       | Vaccination/health info    |
| generalTerms       | Rich Text | No       | Other terms                |
| importantNotes     | Rich Text | No       | Critical information       |

---

### **shared.seo**

SEO metadata component.

#### Fields:

| Field Name      | Type           | Required | Notes                           |
| --------------- | -------------- | -------- | ------------------------------- |
| metaTitle       | Text           | No       | SEO title (max 60 chars)        |
| metaDescription | Text           | No       | SEO description (max 160 chars) |
| keywords        | Text           | No       | Comma-separated keywords        |
| canonicalURL    | Text           | No       | Canonical URL                   |
| metaImage       | Media (Single) | No       | Social share image              |
| metaRobots      | Text           | No       | E.g., "index, follow"           |

---

## Supporting Content Types

### 2. **Destination** (Collection Type)

Destinations/locations for offers.

#### Fields:

| Field Name  | Type                   | Required | Notes                    |
| ----------- | ---------------------- | -------- | ------------------------ |
| name        | Text                   | Yes      | E.g., "Bali", "Istanbul" |
| slug        | UID                    | Yes      | Auto-generated           |
| description | Rich Text              | No       | Destination description  |
| country     | Text                   | Yes      | Country name             |
| region      | Text                   | No       | Region/area              |
| coverImage  | Media (Single)         | Yes      | Main image               |
| gallery     | Media (Multiple)       | No       | Additional images        |
| latitude    | Decimal                | No       | For maps                 |
| longitude   | Decimal                | No       | For maps                 |
| featured    | Boolean                | No       | Default: false           |
| popular     | Boolean                | No       | Default: false           |
| attractions | Component (repeatable) | No       | `destination.attraction` |
| seo         | Component              | No       | `shared.seo`             |

---

### **destination.attraction**

Points of interest in a destination.

#### Fields:

| Field Name  | Type           | Required | Notes                                                                      |
| ----------- | -------------- | -------- | -------------------------------------------------------------------------- |
| name        | Text           | Yes      | Attraction name                                                            |
| description | Text           | No       | Brief description                                                          |
| category    | Enumeration    | No       | `historical`, `cultural`, `natural`, `entertainment`, `religious`, `other` |
| image       | Media (Single) | No       | Attraction image                                                           |

---

### 3. **Tag** (Collection Type)

Tags for categorizing offers.

#### Fields:

| Field Name  | Type | Required | Notes            |
| ----------- | ---- | -------- | ---------------- |
| name        | Text | Yes      | Tag name         |
| slug        | UID  | Yes      | Auto-generated   |
| description | Text | No       | Tag description  |
| color       | Text | No       | Hex color for UI |

---

## Relationships

```
Offer → Destination (Many-to-One)
Offer → Tag (Many-to-Many)
```

---

## Example Data Structure

### Sample Offer JSON:

```json
{
  "title": "Bali Honeymoon Package - November",
  "slug": "bali-honeymoon-package-november",
  "shortDescription": "Romantic 7-day honeymoon in Bali with luxury resorts and private tours",
  "destination": 1,
  "offerType": "package",
  "validFrom": "2024-11-01",
  "validTo": "2024-11-30",
  "month": "november",
  "year": 2024,
  "featured": true,
  "status": "active",
  "pricing": [
    {
      "label": "Double Occupancy",
      "basePrice": 2500,
      "currency": "USD",
      "priceType": "per_person",
      "discountedPrice": 2200,
      "minPeople": 2,
      "maxPeople": 2
    }
  ],
  "inclusions": [
    {
      "title": "7 nights accommodation",
      "description": "5-star resort with ocean view",
      "icon": "hotel"
    },
    {
      "title": "Daily breakfast",
      "icon": "meal"
    },
    {
      "title": "Airport transfers",
      "icon": "transport"
    }
  ],
  "accommodation": [
    {
      "hotelName": "The Mulia Bali",
      "hotelCategory": "5_star",
      "roomType": "Ocean View Suite",
      "nights": 7,
      "boardType": "bed_breakfast"
    }
  ]
}
```

---

## Implementation Notes

### Strapi Configuration:

1. **Permissions**: Set appropriate user roles and permissions for creating/editing offers
2. **Draft/Publish**: Enable draft/publish system for offer approval workflow
3. **Localization**: Enable i18n plugin if you need multi-language support
4. **Media Library**: Configure upload providers (local/S3/Cloudinary)
5. **API Tokens**: Create API tokens for frontend consumption

### Frontend Integration:

- Use Strapi REST or GraphQL API
- Implement filtering by:
  - Destination
  - Date range (validFrom/validTo)
  - Price range
  - Tags
  - Offer type
- Enable search functionality on title, description, destination

### Performance Optimization:

- Add database indexes on: `slug`, `status`, `validFrom`, `validTo`, `destination`
- Enable API caching for frequently accessed offers
- Use pagination for offer listings
- Optimize images through Strapi's image processing

---

## Migration Strategy

1. **Phase 1**: Create all components first
2. **Phase 2**: Create Destination and Tag content types
3. **Phase 3**: Create Offer content type with all relations
4. **Phase 4**: Import existing PDF data into structured format
5. **Phase 5**: Test and validate all relationships

---

## Additional Recommendations

### Plugins to Consider:

- **SEO Plugin**: For better SEO management
- **Sitemap Plugin**: Auto-generate XML sitemaps
- **Email Plugin**: For booking confirmations
- **Upload Plugin**: Enhanced media management
- **GraphQL**: If using GraphQL on frontend

### Custom Fields:

- Consider adding a `viewCount` field to track popular offers
- Add `bookingCount` to show how many times offer was booked
- Implement `availableSeats` for limited capacity offers
- Add `minimumNotice` for booking deadline requirements

---

## Validation Rules

Recommended validations:

- `validTo` must be after `validFrom`
- `discountedPrice` must be less than `basePrice`
- `minPeople` must be less than or equal to `maxPeople`
- `slug` must be unique
- Email format validation for contact fields
- Phone number format validation
- Date validations for past dates

---

## Notes for Developers

1. All dates should be stored in UTC
2. Prices should use Decimal type to avoid floating-point precision issues
3. Use enumerations for consistent data entry
4. Component repeatable fields allow for flexible data structures
5. Keep component granularity balanced - not too small, not too large
6. Consider adding a `metadata` JSON field for any custom data not covered by schema
7. Use lifecycle hooks in Strapi for auto-population of fields (e.g., slug generation)
8. Implement proper image size constraints and formats

---

This schema provides a solid foundation that can be extended based on specific business requirements.
