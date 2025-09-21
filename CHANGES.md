# Argos – Changes in this Fork

This fork is based on [p-e-w/argos](https://github.com/p-e-w/argos) and adds several features not available in upstream.  
The goal is to stay compatible with existing scripts while making the extension more robust and convenient in daily use.

---

## New Features

### 1. Preserve Submenu State
- **Problem:** On every refresh (`updateInterval`), the entire menu was rebuilt, causing all open submenus to collapse.
- **Solution:**  
  - Open submenus are captured before `removeAll()` and restored after rebuilding.  
  - Script output now supports an `id` property for submenu headers:  

    ```text
    -- Submenu Header | id=connection
    ---- Item A
    ---- Item B
    ```

  - Only if `id` is set will the state be stored and restored.  
  - No `id` → no restore (backward compatible).

- **Technical details:**  
  - A new helper class `submenu_state.js` manages capture/restore of submenu states.  
  - Internally, `GLib.idle_add` is used to ensure restoration happens after the rebuild, avoiding race conditions.

---

## Internal Changes

- Code for tracking/restoring submenu state was moved out of `button.js` into a **dedicated utility class**.  
- Refactored parts of the menu update logic for better maintainability.  

---

## Known Limitations

- Only one submenu can be open at a time (GNOME Shell limitation).  
- Restoring an open submenu causes a short re-animation (visible “flicker”) because the menu is rebuilt.  
- Without an `id` property in the script line, no state is restored.  

---

## Planned Features

- **Re-open after command:** Option to keep the menu open after executing an action, or automatically reopen it (e.g., after toggling the kill switch).  
- **Fixed minimum width:** Option to prevent the menu width from jumping when expanding/collapsing submenus.  

---
