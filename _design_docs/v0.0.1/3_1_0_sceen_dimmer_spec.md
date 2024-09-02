# Screen Dimmer Module Specification

## Purpose
The Screen Dimmer Module is responsible for dimming the screen at regular intervals to remind users to take breaks and rest their eyes. It implements the 20-20-20 rule: every 20 minutes, look at something 20 feet away for 20 seconds.

## Core Functionality
1. Screen Dimming
   - Dim the screen to a user-configurable opacity level
   - Maintain dimming for a specified duration (default 20 seconds)
   - Restore screen to normal brightness after the break

2. Timer Management
   - Maintain a countdown timer for break intervals
   - Trigger dimming when the timer reaches zero
   - Reset timer after each break

3. User Interaction
   - Allow users to skip the current break
   - Provide a way to temporarily disable/enable breaks

## OS-Specific Implementations

### Windows
- Use the Windows API (via winapi crate) to control screen brightness
- Implement using SetDeviceGammaRamp or similar function

### macOS
- Utilize Core Graphics framework (via core-graphics crate)
- Implement using CGDisplayCreateUUIDFromDisplayID and CGSetDisplayTransferByTable or similar functions

### Linux
- Use X11 or Wayland protocols (via x11rb or smithay crates)
- Implement using XRRSetCrtcGamma (X11) or similar Wayland protocol methods

## Module Interface
```rust
pub struct ScreenDimmer {
    // Internal fields
}

impl ScreenDimmer {
    pub fn new(config: DimmerConfig) -> Result<Self, Error>;
    pub fn start_timer(&mut self);
    pub fn stop_timer(&mut self);
    pub fn skip_break(&mut self);
    pub fn is_dimmed(&self) -> bool;
    pub fn update_config(&mut self, config: DimmerConfig);
}

pub struct DimmerConfig {
    pub interval: Duration,
    pub duration: Duration,
    pub opacity: f32,
}

pub enum Error {
    OsError(String),
    InvalidConfig,
    // Other error types
}
```

## Interaction with Other Modules
- Settings Manager: Retrieve user preferences for dimming interval, duration, and opacity
- Habit Tracker Module: Notify when screen is dimmed to display habit overlay
- System Tray Interface: Provide methods to control dimming (e.g., skip break, disable/enable)

## Error Handling
- Gracefully handle OS-specific errors (e.g., permission issues, API failures)
- Log errors for debugging purposes
- Provide user-friendly error messages when appropriate

## Performance Considerations
- Minimize resource usage while idle
- Ensure smooth transition when dimming/undimming
- Handle multiple monitors efficiently

## Testing Strategy
- Unit tests for timer logic and configuration handling
- Integration tests for OS-specific dimming implementations
- Mock OS APIs for consistent cross-platform testing

## Future Enhancements
- Support for custom break schedules
- Integration with external events (e.g., system idle time, user activity)
- Gradual dimming effects