# Feature Specification: Collapsible Left Toolbar

**Feature Branch**: `001-collapsible-left-toolbar`
**Created**: 2026-05-01
**Status**: Draft  
**Input**: User description: "Move the top horizontal toolbar to a collapsible left toolbar, so the UI will be even less cluttered."

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Access Settings Without Wasting Vertical Space (Priority: P1)

Users want to configure their API settings (base URL, model, API key) without those controls eating up vertical screen real estate. Currently the toolbar spans the full width at the top, pushing the chat area down. Users with smaller screens or those who prefer more chat space find this wasteful.

**Why this priority**: This is the core value proposition of the feature — reclaiming vertical space for the chat content. Without it, the feature has no purpose.

**Independent Test**: Can be fully tested by opening the app and verifying that all settings are accessible from a left-side panel while the chat area uses the full remaining width.

**Acceptance Scenarios**:

1. **Given** the app is loaded in a browser, **When** the user views the interface, **Then** all current toolbar controls (API base URL, model selector, API key, import/export/load models/info buttons, token rate) are accessible from a panel on the left edge of the screen
2. **Given** the left panel is collapsed, **When** the user clicks the toggle control, **Then** the panel slides open revealing all controls, and clicking again collapses it
3. **Given** the left panel is open, **When** the user interacts with any control (typing API base URL, selecting a model, clicking buttons), **Then** the control behaves identically to its current behavior

### User Story 2 - Maintain Full Functionality During Interaction (Priority: P1)

Users must be able to perform every action they currently can — configure API settings, import/export conversations, load models, view info — without any reduction in capability. Moving controls to a sidebar must not change what the app does, only where those controls live.

**Why this priority**: Any loss of functionality would break existing workflows and frustrate users. This is a layout change, not a feature change.

**Independent Test**: Can be tested by performing every action available in the current toolbar (set API URL, select model, enter key, import file, export chat, load models, view info) from the new left panel and confirming each works correctly.

**Acceptance Scenarios**:

1. **Given** the left panel is open, **When** the user types a new API base URL and clicks "Load models", **Then** models are fetched from the entered URL exactly as before
2. **Given** the left panel is open, **When** the user clicks "Import" and selects a JSON file, **Then** the conversation loads correctly
3. **Given** the left panel is open, **When** the user clicks "Export", **Then** the conversation downloads as a timestamped JSON file

### User Story 3 - Smooth Panel Toggle with Minimal Visual Disruption (Priority: P2)

Users expect the panel to toggle smoothly without jarring layout shifts. When the panel opens or closes, the chat area should reflow naturally without flickering or content jumping.

**Why this priority**: A janky toggle would make the feature feel broken even if functionally correct. The smoothness of the interaction is part of the user experience.

**Independent Test**: Can be tested by rapidly toggling the panel open and closed and observing that the transition is smooth, the chat area reflows without flicker, and no content is clipped or overlapped.

**Acceptance Scenarios**:

1. **Given** the left panel is in any state (open or closed), **When** the user toggles it, **Then** the transition completes within 300ms with smooth easing
2. **Given** the left panel is opening, **When** the chat area resizes, **Then** the reflow happens without visual flicker or content overlap
3. **Given** the user has scrolled the chat history, **When** they toggle the panel, **Then** their scroll position is preserved

### Edge Cases

- What happens when the browser window is narrower than 600px? The left panel should adapt gracefully (e.g., become a full-height overlay or auto-collapse on very narrow viewports).
- How does the app handle rapid successive toggle clicks during an animation? The animation should queue or be interruptible without breaking.
- What happens if the user has many long model names loaded? The panel must accommodate wide content without clipping.
- How does the API key input behave when the panel is collapsed? The value must persist and be accessible when reopened.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: System MUST relocate all current top-bar controls (API base URL input, model selector, API key input, import/export/load models/info buttons, token rate display) into a panel positioned along the left edge of the viewport
- **FR-002**: System MUST provide a toggle control that opens and closes the left panel with a smooth animated transition
- **FR-003**: System MUST preserve all existing control functionality — every interaction available in the current toolbar must work identically from the new left panel
- **FR-004**: System MUST allow the chat area to use the full remaining viewport width when the panel is collapsed, maximizing vertical space for conversation content
- **FR-005**: System MUST persist all control values (API base URL, API key, selected model) across panel open/close cycles and page reloads, exactly as current behavior
- **FR-006**: System MUST maintain the brand/title and promo link in the left panel or a fixed position that remains visible regardless of panel state
- **FR-007**: System MUST adapt the layout for narrow viewports (below 600px) without breaking functionality or readability

### Key Entities

- **Left Panel**: A vertical container along the left edge holding all configuration controls; toggleable open/closed state; smooth animated transition
- **Toggle Control**: A button or icon that triggers panel open/close; visible at all times regardless of panel state
- **Chat Area**: The main conversation display that expands to fill available width when panel is collapsed

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Users can access all configuration controls from the left panel without scrolling within the panel on standard desktop viewports (1280px+ width)
- **SC-002**: The chat area gains at least 200px of additional vertical space when the left panel is collapsed, compared to the current top-bar layout
- **SC-003**: Panel toggle animation completes within 300ms with no visual flicker or content overlap during reflow
- **SC-004**: All existing toolbar actions (API configuration, model loading, import/export, info view) function identically from the left panel with zero loss of capability

## Assumptions

- The existing control values (API base URL, API key, selected model) are already persisted via localStorage or similar — this feature does not need to change persistence logic
- Users primarily interact with settings before starting a conversation and rarely need to adjust them mid-chat, making a collapsible panel appropriate
- The promo link ("PromptInjection.net") should remain visible — it will move into the left panel or stay in a fixed header position
- All current CSS custom properties (CSS variables) for colors and spacing will be reused; no new design tokens are introduced
- Mobile users on very narrow screens may see the panel behave differently (overlay mode), but this is acceptable as an edge case for v1
