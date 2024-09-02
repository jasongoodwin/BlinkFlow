project_setup:
  - name: Initialize Rust project
    command: "Create a new Rust project named 'habit_tracker_app' using Cargo. Show me the command to run and the expected output."

  - name: Set up version control
    commands:
      - "Initialize a git repository in the project folder. Show me the command."
      - "Create a .gitignore file for Rust projects. Include entries for target/, Cargo.lock, and any IDE-specific files."

  - name: Configure Cargo.toml
    commands:
      - "Open the Cargo.toml file and update the package name to 'habit_tracker_app'."
      - "Add a description field to Cargo.toml: 'A habit tracking application with screen break reminders'."
      - "Set the version in Cargo.toml to '0.1.0'."
      - "Add 'Jason Goodwin' as the author in Cargo.toml."
      - "Set the edition in Cargo.toml to '2021'."

  - name: Add dependencies
    commands:
      - "Add the iced dependency to Cargo.toml with version '0.9'."
      - "Add the rusqlite dependency to Cargo.toml with version '0.29' and the feature 'bundled'."
      - "Add the tray-icon dependency to Cargo.toml with version '0.5'."
      - "Add the serde dependency to Cargo.toml with version '1.0' and features ['derive']."
      - "Add the tokio dependency to Cargo.toml with version '1.0' and features ['full']."
      - "Add the log dependency to Cargo.toml with version '0.4'."
      - "Add the env_logger dependency to Cargo.toml with version '0.10'."
      - "Add the chrono dependency to Cargo.toml with version '0.4' and features ['serde']."
      - "Add the config dependency to Cargo.toml with version '0.13'."

  - name: Create basic project structure
    commands:
      - "Create a directory named 'src' in the project root if it doesn't exist already."
      - "Inside the 'src' directory, create a file named 'main.rs'."
      - "In src/main.rs, add a basic main function that prints 'Hello, Habit Tracker!' to the console."

  - name: Set up logging
    commands:
      - "In src/main.rs, add the necessary use statements for the log and env_logger crates."
      - "In the main function in src/main.rs, initialize the env_logger."
      - "Add a log info! macro call in the main function to log 'Habit Tracker application started'."

  - name: Create module structure
    commands:
      - "Create a directory named 'settings' inside the 'src' directory."
      - "Create a file named 'mod.rs' inside the 'src/settings' directory."
      - "Create a directory named 'habit_tracker' inside the 'src' directory."
      - "Create a file named 'mod.rs' inside the 'src/habit_tracker' directory."
      - "Create a directory named 'screen_dimmer' inside the 'src' directory."
      - "Create a file named 'mod.rs' inside the 'src/screen_dimmer' directory."

  - name: Set up CI/CD pipeline
    commands:
      - "Create a directory named '.github' in the project root."
      - "Create a directory named 'workflows' inside the '.github' directory."
      - "Create a file named 'ci.yml' inside the '.github/workflows' directory."
      - "In .github/workflows/ci.yml, add a basic GitHub Actions workflow that builds and tests the project on push and PR events."

  - name: Initial commit
    commands:
      - "Stage all the files we've created using git add."
      - "Commit the changes with the message 'Initial project setup'."

  - name: Verify setup
    command: "Run 'cargo build' to ensure the project compiles without errors. Show me the command and the expected output."

settings_module_implementation:
  - name: Create Settings struct
    commands:
      - "In src/settings/mod.rs, create a struct named Settings with the following fields: id (u32), break_interval (u32), break_duration (u32), dim_percentage (u8)."
      - "Derive Debug, Clone, and PartialEq traits for the Settings struct."

  - name: Implement Settings methods
    commands:
      - "Add a new() method to the Settings struct that creates a default Settings instance."
      - "Implement a validate() method for Settings that checks if all fields are within acceptable ranges."

  - name: Create SettingsRepository trait
    commands:
      - "In src/settings/mod.rs, create a trait named SettingsRepository with the following method signatures: 
         - save(&self, settings: &Settings) -> Result<(), SettingsError>
         - get(&self) -> Result<Settings, SettingsError>
         - update(&self, settings: &Settings) -> Result<(), SettingsError>
         - delete(&self) -> Result<(), SettingsError>"

  - name: Create SettingsError enum
    commands:
      - "In src/settings/mod.rs, create an enum named SettingsError with variants for common error cases (e.g., NotFound, ValidationError, DatabaseError)."
      - "Implement the std::error::Error trait for SettingsError."

  - name: Implement use cases
    commands:
      - "Create a function named create_settings that takes a SettingsRepository and Settings as parameters, validates the settings, and calls the repository's save method."
      - "Create a function named get_settings that takes a SettingsRepository as a parameter and returns the current settings."
      - "Create a function named update_settings that takes a SettingsRepository and Settings as parameters, validates the new settings, and calls the repository's update method."
      - "Create a function named delete_settings that takes a SettingsRepository as a parameter and calls the repository's delete method."

  - name: Add tests
    commands:
      - "Create a new module named tests at the bottom of src/settings/mod.rs using #[cfg(test)]."
      - "In the tests module, add a test for Settings::new() to ensure it creates default settings correctly."
      - "Add a test for Settings::validate() to check it correctly identifies valid and invalid settings."
      - "Create a mock implementation of SettingsRepository for testing purposes."
      - "Add tests for each use case function (create_settings, get_settings, update_settings, delete_settings) using the mock repository."

  - name: Update main.rs
    commands:
      - "In src/main.rs, add a mod settings; statement to include the settings module."
      - "In the main function, create a default Settings instance and print it to the console to verify the module is working correctly."

  - name: Verify implementation
    command: "Run 'cargo test' to ensure all new tests pass and 'cargo run' to verify the Settings struct is printed correctly. Show me the expected output."

habit_tracker_module_implementation:
  - name: Create Habit struct
    commands:
      - "In src/habit_tracker/mod.rs, create a struct named Habit with the following fields: id (u32), name (String), description (Option<String>), frequency (Frequency), created_at (chrono::DateTime<chrono::Utc>)."
      - "Derive Debug, Clone, and PartialEq traits for the Habit struct."

  - name: Create Frequency enum
    commands:
      - "In src/habit_tracker/mod.rs, create an enum named Frequency with variants: Daily, Weekly(Vec<chrono::Weekday>), Monthly(Vec<u8>)."
      - "Derive Debug, Clone, and PartialEq traits for the Frequency enum."

  - name: Implement Habit methods
    commands:
      - "Add a new() method to the Habit struct that creates a new Habit instance with a given name, description, and frequency."
      - "Implement a validate() method for Habit that checks if all fields are valid (e.g., name is not empty, frequency is valid)."

  - name: Create HabitRepository trait
    commands:
      - "In src/habit_tracker/mod.rs, create a trait named HabitRepository with the following method signatures:
         - save(&self, habit: &Habit) -> Result<(), HabitError>
         - get(&self, id: u32) -> Result<Habit, HabitError>
         - get_all(&self) -> Result<Vec<Habit>, HabitError>
         - update(&self, habit: &Habit) -> Result<(), HabitError>
         - delete(&self, id: u32) -> Result<(), HabitError>
         - get_habits_for_date(&self, date: chrono::NaiveDate) -> Result<Vec<Habit>, HabitError>"

  - name: Create HabitError enum
    commands:
      - "In src/habit_tracker/mod.rs, create an enum named HabitError with variants for common error cases (e.g., NotFound, ValidationError, DatabaseError)."
      - "Implement the std::error::Error trait for HabitError."

  - name: Implement use cases
    commands:
      - "Create a function named create_habit that takes a HabitRepository and Habit as parameters, validates the habit, and calls the repository's save method."
      - "Create a function named get_habit that takes a HabitRepository and habit id as parameters and returns the habit if found."
      - "Create a function named update_habit that takes a HabitRepository and Habit as parameters, validates the habit, and calls the repository's update method."
      - "Create a function named delete_habit that takes a HabitRepository and habit id as parameters and calls the repository's delete method."
      - "Create a function named get_habits_for_today that takes a HabitRepository as a parameter and returns all habits due for the current date."

  - name: Add tests
    commands:
      - "Create a new module named tests at the bottom of src/habit_tracker/mod.rs using #[cfg(test)]."
      - "In the tests module, add a test for Habit::new() to ensure it creates a habit correctly."
      - "Add a test for Habit::validate() to check it correctly identifies valid and invalid habits."
      - "Create a mock implementation of HabitRepository for testing purposes."
      - "Add tests for each use case function (create_habit, get_habit, update_habit, delete_habit, get_habits_for_today) using the mock repository."

  - name: Update main.rs
    commands:
      - "In src/main.rs, add a mod habit_tracker; statement to include the habit_tracker module."
      - "In the main function, create a sample Habit instance and print it to the console to verify the module is working correctly."

  - name: Verify implementation
    command: "Run 'cargo test' to ensure all new tests pass and 'cargo run' to verify the Habit struct is printed correctly. Show me the expected output."

screen_dimmer_module_implementation:
  - name: Create ScreenDimmer trait
    commands:
      - "In src/screen_dimmer/mod.rs, create a trait named ScreenDimmer with the following method signatures:
         - dim(&self, percentage: u8) -> Result<(), ScreenDimmerError>
         - undim(&self) -> Result<(), ScreenDimmerError>
         - get_current_dim_level(&self) -> Result<u8, ScreenDimmerError>"

  - name: Create ScreenDimmerError enum
    commands:
      - "In src/screen_dimmer/mod.rs, create an enum named ScreenDimmerError with variants for common error cases (e.g., InvalidPercentage, SystemError)."
      - "Implement the std::error::Error trait for ScreenDimmerError."

  - name: Create DimmingSchedule struct
    commands:
      - "In src/screen_dimmer/mod.rs, create a struct named DimmingSchedule with fields: interval_minutes (u32), duration_seconds (u32), dim_percentage (u8)."
      - "Derive Debug, Clone, and PartialEq traits for the DimmingSchedule struct."

  - name: Implement DimmingSchedule methods
    commands:
      - "Add a new() method to DimmingSchedule that creates a new instance with given parameters."
      - "Implement a validate() method for DimmingSchedule that checks if all fields are within acceptable ranges."

  - name: Create ScreenDimmerService trait
    commands:
      - "In src/screen_dimmer/mod.rs, create a trait named ScreenDimmerService with the following method signatures:
         - start_schedule(&self, schedule: DimmingSchedule) -> Result<(), ScreenDimmerError>
         - stop_schedule(&self) -> Result<(), ScreenDimmerError>
         - skip_next_dim(&self) -> Result<(), ScreenDimmerError>
         - get_active_schedule(&self) -> Result<Option<DimmingSchedule>, ScreenDimmerError>"

  - name: Implement MockScreenDimmer
    commands:
      - "Create a struct named MockScreenDimmer that implements the ScreenDimmer trait."
      - "Implement the dim, undim, and get_current_dim_level methods for MockScreenDimmer, using internal state to track the current dim level."

  - name: Implement use cases
    commands:
      - "Create a function named start_dimming_schedule that takes a ScreenDimmerService and DimmingSchedule as parameters, validates the schedule, and calls the service's start_schedule method."
      - "Create a function named stop_dimming_schedule that takes a ScreenDimmerService as a parameter and calls the service's stop_schedule method."
      - "Create a function named skip_next_dim_event that takes a ScreenDimmerService as a parameter and calls the service's skip_next_dim method."

  - name: Add tests
    commands:
      - "Create a new module named tests at the bottom of src/screen_dimmer/mod.rs using #[cfg(test)]."
      - "In the tests module, add a test for DimmingSchedule::new() to ensure it creates a schedule correctly."
      - "Add a test for DimmingSchedule::validate() to check it correctly identifies valid and invalid schedules."
      - "Create a mock implementation of ScreenDimmerService for testing purposes."
      - "Add tests for each use case function (start_dimming_schedule, stop_dimming_schedule, skip_next_dim_event) using the mock service."

  - name: Update main.rs
    commands:
      - "In src/main.rs, add a mod screen_dimmer; statement to include the screen_dimmer module."
      - "In the main function, create a sample DimmingSchedule instance and print it to the console to verify the module is working correctly."

  - name: Verify implementation
    command: "Run 'cargo test' to ensure all new tests pass and 'cargo run' to verify the DimmingSchedule struct is printed correctly. Show me the expected output."