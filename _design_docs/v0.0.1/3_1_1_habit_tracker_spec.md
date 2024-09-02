# Habit Tracker Module Specification

## Purpose
The Habit Tracker Module is responsible for managing user-defined habits, tracking their completion status, and displaying them during screen break overlays. It provides the core functionality for habit management and progress tracking.

## Core Functionality
1. Habit Management
   - Create new habits with customizable attributes
   - Read/retrieve existing habits
   - Update habit details and progress
   - Delete habits

2. Progress Tracking
   - Track daily completion status for each habit
   - Calculate and store streak information
   - Generate summary statistics (e.g., completion rates, longest streaks)

3. Overlay Display
   - Prepare habit data for display during screen breaks
   - Prioritize habits based on completion status and user-defined importance

4. Data Persistence
   - Save habit data to local storage
   - Load habit data from local storage
   - Handle data migration for app updates

## Module Interface
```rust
pub struct HabitTracker {
    // Internal fields
}

impl HabitTracker {
    pub fn new() -> Result<Self, Error>;
    pub fn add_habit(&mut self, habit: Habit) -> Result<HabitId, Error>;
    pub fn get_habit(&self, id: HabitId) -> Option<&Habit>;
    pub fn update_habit(&mut self, id: HabitId, updates: HabitUpdates) -> Result<(), Error>;
    pub fn delete_habit(&mut self, id: HabitId) -> Result<(), Error>;
    pub fn mark_completed(&mut self, id: HabitId, date: NaiveDate) -> Result<(), Error>;
    pub fn get_habits_for_overlay(&self) -> Vec<HabitSummary>;
    pub fn get_progress_summary(&self, timeframe: Timeframe) -> ProgressSummary;
}

pub struct Habit {
    pub id: HabitId,
    pub name: String,
    pub description: Option<String>,
    pub frequency: Frequency,
    pub importance: Importance,
    pub created_at: DateTime<Utc>,
}

pub struct HabitSummary {
    pub id: HabitId,
    pub name: String,
    pub completed_today: bool,
    pub streak: u32,
}

pub enum Frequency {
    Daily,
    Weekly(Vec<Weekday>),
    Monthly(Vec<u8>),
}

pub enum Importance {
    Low,
    Medium,
    High,
}

pub enum Timeframe {
    Day,
    Week,
    Month,
    Year,
    AllTime,
}

pub enum Error {
    StorageError(String),
    InvalidInput(String),
    HabitNotFound,
    // Other error types
}
```

## Data Model
- Use SQLite for local storage
- Define schema for habits, daily logs, and summary statistics

## Interaction with Other Modules
- Screen Dimmer Module: Provide habit data for overlay display when screen is dimmed
- Settings Manager: Retrieve user preferences for habit display and notifications
- System Tray Interface: Provide quick access to mark habits as completed

## Error Handling
- Gracefully handle storage errors (e.g., disk full, permission issues)
- Validate user input to prevent invalid habit data
- Log errors for debugging purposes
- Provide user-friendly error messages when appropriate

## Performance Considerations
- Optimize database queries for quick retrieval of habit data
- Implement caching mechanisms for frequently accessed data
- Use efficient data structures for in-memory habit representation

## Testing Strategy
- Unit tests for core habit management logic
- Integration tests for data persistence layer
- Mock database for testing storage operations
- Property-based testing for habit calculations (e.g., streak computation)

## Future Enhancements
- Habit categories or tags for better organization
- Habit dependencies (e.g., completing one habit unlocks another)
- Data export/import functionality
- Sync capabilities with cloud storage or between devices