# Requirements Document

## Introduction

This feature will create an automated n8n workflow that discovers and filters crypto/Web3 job opportunities and new blockchain projects, then sends personalized notifications to a Telegram group. The system will continuously monitor multiple data sources, apply intelligent filtering based on user skills, and deliver daily digests of relevant opportunities.

## Requirements

### Requirement 1

**User Story:** As a Web3 developer, I want to automatically receive filtered crypto job listings that match my skills, so that I don't miss relevant opportunities while focusing on my current work.

#### Acceptance Criteria

1. WHEN the system runs daily THEN it SHALL fetch job listings from at least 5 major crypto job boards (CryptoJobsList, Web3.career, Remote3.co, CryptocurrencyJobs.co, crypto.jobs), add other crypto jobs platform too.
2. WHEN job listings are fetched THEN the system SHALL extract job title, description, company, location, and application link
3. WHEN job descriptions are analyzed THEN the system SHALL match them against predefined skills from skills.md file
4. WHEN a job matches at least 3 skills THEN the system SHALL include it in the notification
5. WHEN jobs are filtered THEN the system SHALL format them into readable Telegram messages
6. WHEN the daily digest is ready THEN the system SHALL send it to the specified Telegram group

### Requirement 2

**User Story:** As a crypto investor and developer, I want to be notified about new blockchain projects and token launches, so that I can identify potential collaboration or investment opportunities early.

#### Acceptance Criteria

1. WHEN the system runs daily THEN it SHALL fetch new project data from CoinGecko, CoinMarketCap, DeFiLlama, and Dexscreener APIs
2. WHEN new tokens are discovered THEN the system SHALL extract project name, description, social links, and launch date
3. WHEN project descriptions are analyzed THEN the system SHALL filter based on technology stack and project type relevance
4. WHEN relevant projects are found THEN the system SHALL include project details in the Telegram notification
5. WHEN API rate limits are encountered THEN the system SHALL handle errors gracefully and retry appropriately

### Requirement 3

**User Story:** As a busy professional, I want to receive consolidated daily/ twice daily or three times daily notifications or how you think its better and alerts, so that I can review opportunities at my preferred time without interruption.

#### Acceptance Criteria

1. WHEN the workflow is triggered THEN it SHALL run once daily at 9:00 AM
2. WHEN multiple data sources are processed THEN the system SHALL merge all results into a single notification
3. WHEN the notification is sent THEN it SHALL include both job opportunities and new projects in organized sections
4. WHEN no relevant matches are found THEN the system SHALL send a brief "no matches today" message
5. WHEN the system encounters errors THEN it SHALL log them and attempt to send partial results if available

### Requirement 4

**User Story:** As a user managing the system, I want the workflow to be maintainable and configurable, so that I can easily update skills, add new data sources, or modify filtering criteria.

#### Acceptance Criteria

1. WHEN skills need updating THEN the system SHALL read from an external skills.md file that can be easily modified
2. WHEN new job boards are added THEN the system SHALL support modular addition of new scraping targets
3. WHEN API keys expire THEN the system SHALL provide clear error messages indicating which service needs attention
4. WHEN the workflow fails THEN it SHALL log detailed error information for troubleshooting
5. WHEN configuration changes are made THEN the system SHALL validate settings before execution

### Requirement 5

**User Story:** As a privacy-conscious user, I want the system to handle my data securely and respect rate limits, so that my accounts remain in good standing and my information stays protected.

#### Acceptance Criteria

1. WHEN making API requests THEN the system SHALL respect rate limits for each service
2. WHEN storing temporary data THEN the system SHALL not persist sensitive information unnecessarily
3. WHEN sending Telegram messages THEN the system SHALL use secure bot token authentication
4. WHEN scraping websites THEN the system SHALL include appropriate delays and user agent headers
5. WHEN handling errors THEN the system SHALL not expose sensitive configuration details in logs