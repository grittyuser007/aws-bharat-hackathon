# Requirements Document: KalaManager AI

## Introduction

KalaManager AI is a voice-first business management platform designed for rural Indian artisans with limited technical literacy. The system eliminates predatory middlemen by providing an autonomous AI agent that handles marketing, pricing, raw material sourcing, and logistics. The platform enables artisans to manage their craft business entirely through voice interactions in their local dialect, with a simple visual dashboard for key metrics.

## Glossary

- **Artisan**: A rural craftsperson who creates handmade products and uses the KalaManager AI system
- **Voice_Agent**: The AI-powered conversational interface that responds to the "Kalakaar" wake word
- **Transcription_Service**: Amazon Transcribe component that converts spoken dialect to text
- **Speech_Service**: Amazon Polly component that converts text responses to speech
- **Intelligence_Engine**: Amazon Bedrock (Claude 3.5 Sonnet) that processes requests and makes decisions
- **Product_Listing**: A marketplace-ready description of an artisan's product with all required metadata
- **Pricing_Analyzer**: Component that researches market prices and generates recommendations
- **Media_Processor**: AWS Elemental MediaConvert that creates marketing videos from photos
- **Image_Assessor**: Amazon Rekognition that evaluates photo quality
- **Material_Sourcer**: Component that searches suppliers and compares raw material prices
- **Dashboard**: The visual interface showing sales metrics and inventory status
- **Feedback_Analyzer**: Component that performs sentiment analysis on buyer reviews
- **Authentication_Service**: AWS Cognito handling OTP-based user authentication
- **Marketplace**: External e-commerce platform where products are listed (future ONDC integration)
- **Supplier**: External vendor providing raw materials to artisans
- **Order**: A purchase request from a buyer for an artisan's product
- **Inventory**: Current stock levels of raw materials owned by the artisan
- **Dialect**: Regional language variant (Hindi, English, Tamil, Telugu, Bengali, Marathi)
- **Trend**: A popular craft category or product type showing increased market demand
- **Trend_Analyzer**: Component that identifies and tracks popular craft trends from marketplace data

## Requirements

### Requirement 1: Voice-Activated Product Listing

**User Story:** As an artisan, I want to describe my product by speaking in my local dialect, so that I can create marketplace listings without typing or technical knowledge.

#### Acceptance Criteria

1. WHEN an artisan says "Kalakaar", THE Voice_Agent SHALL activate and provide audio confirmation
2. WHEN the Voice_Agent is active, THE Transcription_Service SHALL convert spoken dialect to text with support for Hindi, English, Tamil, Telugu, Bengali, and Marathi
3. WHEN an artisan describes a product, THE Intelligence_Engine SHALL extract material, category, craft story, dimensions, and colors from the description
4. IF extracted information is incomplete, THEN THE Voice_Agent SHALL ask clarifying questions in the artisan's dialect
5. WHEN all required information is collected, THE System SHALL generate a marketplace-ready Product_Listing
6. WHEN a Product_Listing is created, THE System SHALL persist it to storage within 3 seconds
7. THE System SHALL complete the entire product listing process within 3 minutes from wake word activation

### Requirement 2: AI-Powered Pricing Recommendations

**User Story:** As an artisan, I want intelligent pricing suggestions based on market research, so that I can price my products competitively without being exploited.

#### Acceptance Criteria

1. WHEN a Product_Listing is created, THE Pricing_Analyzer SHALL search current market prices for similar products
2. WHEN analyzing prices, THE Pricing_Analyzer SHALL consider material costs, labor time estimates, market demand, and competitor pricing
3. WHEN price analysis is complete, THE Pricing_Analyzer SHALL generate a recommended price range
4. THE Voice_Agent SHALL explain the pricing recommendation in the artisan's dialect with reasoning
5. WHEN an artisan hears the recommendation, THE System SHALL allow them to accept or modify the suggested price through voice commands
6. THE recommended price SHALL be within 10% of actual market rates for similar products
7. WHEN an artisan modifies a price, THE System SHALL persist the final price to the Product_Listing

### Requirement 3: Marketing Video Generation

**User Story:** As an artisan, I want the system to create professional marketing videos from my product photos, so that I can advertise effectively without video editing skills.

#### Acceptance Criteria

1. WHEN an artisan uploads product photos, THE Image_Assessor SHALL evaluate image quality using brightness, focus, and composition metrics
2. IF image quality is below acceptable threshold, THEN THE Voice_Agent SHALL provide spoken guidance for capturing better photos
3. WHEN acceptable photos are uploaded, THE Media_Processor SHALL generate a marketing reel with transitions and text overlays
4. THE Media_Processor SHALL add background music appropriate for craft product marketing
5. THE generated video SHALL be optimized for marketplace advertisement formats
6. WHEN video generation is complete, THE System SHALL make the video available for marketplace upload within 60 seconds
7. THE System SHALL support video generation from a minimum of 3 photos and maximum of 10 photos

### Requirement 4: Raw Material Sourcing and Price Comparison

**User Story:** As an artisan, I want to find the best prices for raw materials through voice commands, so that I can reduce costs and avoid middlemen markup.

#### Acceptance Criteria

1. WHEN an artisan speaks their material needs, THE Material_Sourcer SHALL search multiple supplier websites for current prices
2. WHEN price data is collected, THE Material_Sourcer SHALL generate a comparison showing at least 3 supplier options
3. THE Voice_Agent SHALL present the price comparison as a spoken summary in the artisan's dialect
4. WHEN an artisan approves a supplier choice, THE System SHALL facilitate purchase through supplier integration APIs
5. WHEN materials are purchased, THE System SHALL update the artisan's Inventory records
6. THE Material_Sourcer SHALL track inventory levels for all registered materials
7. WHEN inventory for any material falls below 20% of typical usage, THE System SHALL generate an early warning notification

### Requirement 5: Managerial Dashboard with Voice Navigation

**User Story:** As an artisan, I want a simple visual dashboard showing my earnings and orders, so that I can understand my business status at a glance.

#### Acceptance Criteria

1. WHEN an artisan opens the Dashboard, THE System SHALL display daily earnings in large, prominent text
2. THE Dashboard SHALL show total sales count for the current day
3. THE Dashboard SHALL display a list of pending orders with basic details
4. WHEN viewing pending orders, THE Dashboard SHALL show an order feasibility indicator based on current Inventory
5. THE feasibility indicator SHALL use color coding: green for sufficient materials, yellow for low stock, red for insufficient materials
6. WHEN inventory is insufficient for pending orders, THE System SHALL display an early warning message
7. THE Dashboard SHALL support voice navigation for all features using the Voice_Agent
8. WHEN an artisan speaks a dashboard command, THE System SHALL respond within 2 seconds

### Requirement 6: Buyer Feedback Analysis and Reporting

**User Story:** As an artisan, I want to understand customer feedback in simple terms, so that I can improve my products and service.

#### Acceptance Criteria

1. WHEN buyer reviews are posted on the Marketplace, THE System SHALL collect and store the feedback
2. WHEN feedback is collected, THE Feedback_Analyzer SHALL perform sentiment analysis to categorize reviews as positive, neutral, or negative
3. THE Feedback_Analyzer SHALL identify common themes in feedback such as quality, delivery speed, and packaging
4. WHEN an artisan requests their report card, THE Voice_Agent SHALL provide a spoken summary in their dialect
5. THE spoken summary SHALL include total customer count, positive feedback count, and key improvement areas
6. THE System SHALL generate feedback reports on a weekly basis
7. WHEN critical negative feedback is received, THE System SHALL notify the artisan within 24 hours

### Requirement 7: Multilingual Support Across All Interfaces

**User Story:** As an artisan, I want to use the entire system in my preferred language, so that I can work comfortably without language barriers.

#### Acceptance Criteria

1. THE System SHALL support Hindi, English, Tamil, Telugu, Bengali, and Marathi for all UI text
2. THE Transcription_Service SHALL recognize and process spoken input in all six supported dialects
3. THE Speech_Service SHALL generate voice responses in the artisan's selected language
4. WHEN an artisan changes their language preference, THE System SHALL update all UI elements and voice responses immediately
5. THE Voice_Agent SHALL maintain conversation context when switching between languages mid-session
6. WHEN displaying text in the Dashboard, THE System SHALL use appropriate fonts and character sets for each language
7. THE System SHALL persist language preference across sessions

### Requirement 8: Offline Capability for Critical Features

**User Story:** As an artisan in a rural area with intermittent internet, I want core features to work offline, so that I can continue working during connectivity issues.

#### Acceptance Criteria

1. WHEN internet connectivity is unavailable, THE System SHALL allow viewing of the Dashboard with cached data
2. WHEN offline, THE System SHALL allow recording of voice product descriptions for later processing
3. WHEN offline, THE System SHALL display previously loaded inventory levels and pending orders
4. WHEN connectivity is restored, THE System SHALL automatically sync all offline-recorded data
5. THE System SHALL indicate offline status clearly in the UI
6. WHEN attempting features that require internet while offline, THE System SHALL provide clear voice feedback explaining the limitation
7. THE System SHALL cache the most recent 30 days of dashboard data for offline access

### Requirement 9: Low-Resource Device Optimization

**User Story:** As an artisan using a low-end Android device, I want the app to run smoothly, so that I can use all features without performance issues.

#### Acceptance Criteria

1. THE System SHALL run on Android devices with minimum 3GB RAM
2. THE System SHALL function on devices running Android 8.0 or higher
3. WHEN loading the Dashboard, THE System SHALL display content within 3 seconds on low-end devices
4. THE System SHALL limit data usage to maximum 50MB per day for typical usage patterns
5. WHEN uploading photos, THE System SHALL compress images to reduce data transfer while maintaining acceptable quality
6. THE System SHALL cache frequently accessed data locally to minimize network requests
7. WHEN processing voice commands, THE System SHALL provide audio feedback within 1 second to confirm receipt

### Requirement 10: Simple OTP-Based Authentication

**User Story:** As an artisan with limited literacy, I want to log in using only my phone number and OTP, so that I can access the system without remembering passwords.

#### Acceptance Criteria

1. WHEN an artisan opens the app for the first time, THE Authentication_Service SHALL request their mobile phone number
2. WHEN a phone number is entered, THE Authentication_Service SHALL send a 6-digit OTP via SMS
3. THE OTP SHALL remain valid for 10 minutes after generation
4. WHEN an artisan enters the correct OTP, THE Authentication_Service SHALL create an authenticated session
5. THE authenticated session SHALL remain valid for 30 days without requiring re-authentication
6. WHEN an OTP is incorrect, THE System SHALL allow up to 3 retry attempts before requiring a new OTP
7. THE Voice_Agent SHALL provide spoken instructions for the authentication process in the artisan's selected language

### Requirement 11: Order Fulfillment Tracking

**User Story:** As an artisan, I want to know if I can fulfill my pending orders with current materials, so that I can plan my work and avoid disappointing customers.

#### Acceptance Criteria

1. WHEN an Order is received, THE System SHALL calculate required materials based on the product specification
2. WHEN calculating feasibility, THE System SHALL compare required materials against current Inventory levels
3. THE System SHALL classify each Order as feasible, at-risk, or unfeasible based on material availability
4. WHEN an Order is unfeasible, THE System SHALL identify which specific materials are insufficient
5. THE Dashboard SHALL display feasibility status for all pending Orders
6. WHEN multiple Orders compete for limited materials, THE System SHALL prioritize based on order date
7. WHEN an artisan completes an Order, THE System SHALL automatically deduct used materials from Inventory

### Requirement 12: Craft Trends Discovery and Inspiration

**User Story:** As an artisan, I want to discover trending craft ideas and new techniques, so that I can stay relevant and avoid being phased out by market changes.

#### Acceptance Criteria

1. WHEN an artisan requests trend information, THE Intelligence_Engine SHALL search current marketplace trends and popular craft categories
2. THE System SHALL analyze trending products based on sales volume, search frequency, and seasonal demand
3. WHEN trends are identified, THE Voice_Agent SHALL present them as spoken suggestions in the artisan's dialect
4. THE System SHALL provide examples of trending products with images and descriptions
5. WHEN an artisan expresses interest in a trend, THE System SHALL provide guidance on materials needed and techniques required
6. THE System SHALL update trend information weekly to reflect current market conditions
7. WHEN presenting trends, THE System SHALL prioritize trends that match the artisan's existing skills and available materials
8. THE Dashboard SHALL display a "Trending Now" section with visual cards showing popular craft categories

### Requirement 13: Marketplace Integration Readiness

**User Story:** As a system architect, I want the product listing format to be compatible with ONDC protocol, so that we can integrate with multiple marketplaces in the future.

#### Acceptance Criteria

1. THE Product_Listing SHALL include all fields required by generic e-commerce platforms
2. THE Product_Listing data structure SHALL be extensible to support ONDC protocol fields
3. WHEN generating a Product_Listing, THE System SHALL validate all required fields are present
4. THE System SHALL support export of Product_Listing data in JSON format
5. WHEN marketplace APIs are configured, THE System SHALL support automated listing publication
6. THE Product_Listing SHALL include product images, description, price, dimensions, materials, and category
7. THE System SHALL maintain a mapping between internal product IDs and external marketplace IDs

## Non-Functional Requirements

### Performance

- Voice command response time: < 2 seconds
- Product listing creation: < 3 minutes end-to-end
- Dashboard load time: < 3 seconds on low-end devices
- Video generation: < 60 seconds for 3-10 photos
- Data usage: < 50MB per day for typical usage

### Reliability

- System uptime: 99.5% availability
- Offline mode: Core features accessible without internet
- Data sync: Automatic when connectivity restored
- Order fulfillment accuracy: > 95%

### Security

- OTP-based authentication only
- Session validity: 30 days
- Data encryption in transit and at rest
- PII protection for artisan information

### Usability

- Zero technical literacy required
- Voice-first interaction model
- Large, clear visual elements
- Multilingual support for 6 Indian languages
- Audio feedback for all actions

### Scalability

- Support for 100,000+ concurrent artisan users
- Handle 1 million+ product listings
- Process 10,000+ voice commands per minute
