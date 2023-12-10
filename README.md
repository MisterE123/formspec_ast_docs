# formspec_ast_docs
AI generated documentation for formspec_ast

The `formspec_ast` mod for Minetest is a library designed to simplify the modification of formspecs. Here's an overview of its API and some examples of how to use it:

### API Overview

- `formspec_ast.parse(formspec_string)`: Parses a formspec string and returns an Abstract Syntax Tree (AST).
- `formspec_ast.unparse(tree)`: Converts the provided AST back into a formspec string.
- `formspec_ast.interpret(string_or_tree)`: Returns a formspec string after optionally parsing and unparsing the provided formspec.
- `formspec_ast.walk(tree, optional_container_set)`: Returns an iterator for all nodes in a tree, including those inside containers.
- `formspec_ast.find(tree, node_type)`: Returns nodes of a specific type from the tree.
- `formspec_ast.get_element_by_name(tree, name)`: Finds the first element in the tree with the specified name.
- `formspec_ast.get_elements_by_name(tree, name)`: Retrieves all elements with the given name.
- `formspec_ast.apply_offset(tree, x, y)`: Shifts all elements in the tree by the specified x and y offsets.
- `formspec_ast.flatten(tree)`: Removes all containers and adjusts elements that were in containers accordingly.
- `formspec_ast.show_formspec(player_or_name, formname, formspec)`: Similar to `minetest.show_formspec`, but also accepts player objects and processes the formspec through `formspec_ast.interpret`.
- `formspec_ast.safe_parse(string_or_tree)`: Parses a formspec while removing elements that may crash the client.
- `formspec_ast.safe_interpret(string_or_tree)`: Safe version of `formspec_ast.interpret`.
- `formspec_ast.formspec_escape(text)`: Escapes text for use in formspecs, similar to `minetest.formspec_escape`.

### Examples

#### Parsing a Formspec

```lua
tree = formspec_ast.parse('size[5,2] '
    .. 'style[name;bgcolor=blue;textcolor=yellow]'
    .. 'container[1,1]'
    .. '    label[0,0;Containers are fun]'
    .. '    container[-1,-1]'
    .. '        button[0.5,0;4,1;name;Label]'
    .. '    container_end[]'
    .. '    label[0,1;Nested containers work too.]'
    .. 'container_end[]'
    .. ' image[0,1;1,1;air.png]')
```

#### Flattening a Formspec

```lua
print(dump(formspec_ast.flatten(tree)))
```

#### Unparsing a Formspec

```lua
print(formspec_ast.unparse(tree))
print(formspec_ast.unparse(formspec_ast.flatten(tree)))
```

### Usage

To use `formspec_ast`, you typically start by parsing an existing formspec string into an AST. This AST can then be manipulated programmatically, allowing for dynamic changes to the formspec layout and content. Once the desired modifications are made, you can unparse the AST back into a formspec string for use in Minetest.

This mod is particularly useful for developers who need to create or modify complex formspecs dynamically, providing a more structured and manageable approach than manipulating raw formspec strings.

Creating comprehensive documentation for each element in the `formspec_ast` tree, similar to the Minetest formspec API, involves detailing the usage, parameters, and examples for each element. Let's start with some of the elements:

### 1. `anchor`
- **Usage**: Defines the anchor point within the formspec.
- **Parameters**:
  - `x` (number): X-coordinate of the anchor point.
  - `y` (number): Y-coordinate of the anchor point.
- **Example**:
  ```lua
  { type = "anchor", x = 0.5, y = 0.5 }
  ```

### 2. `animated_image`
- **Usage**: Displays an animated image.
- **Parameters**:
  - `x`, `y` (number): Position of the image.
  - `w`, `h` (number): Width and height of the image.
  - `name` (string): Element name for event handling.
  - `texture_name` (string): Image resource name.
  - `frame_count`, `frame_duration`, `frame_start` (number): Animation parameters.
  - `middle_x`, `middle_y`, `middle_x2`, `middle_y2` (number, optional): Parameters for 9-sliced mode.
- **Example**:
  ```lua
  {
    type = "animated_image",
    x = 1, y = 1, w = 2, h = 2,
    name = "anim_img",
    texture_name = "example.png",
    frame_count = 10,
    frame_duration = 30,
    frame_start = 1
  }
  ```

### 3. `background`
- **Usage**: Sets the background image of the formspec.
- **Parameters**:
  - `x`, `y` (number): Position of the background.
  - `w`, `h` (number): Width and height of the background.
  - `texture_name` (string): Image resource name.
  - `auto_clip` (boolean, optional): If true, clips the background to the formspec size.
- **Example**:
  ```lua
  { type = "background", x = 0, y = 0, w = 5, h = 5, texture_name = "bg.png" }
  ```

### 4. `background9`
- **Usage**: Defines a 9-sliced background.
- **Parameters**:
  - `x`, `y`, `w`, `h` (number): Position and size of the background.
  - `texture_name` (string): Image resource name.
  - `auto_clip` (boolean, optional): Enables clipping to the formspec size.
  - `middle_x`, `middle_y`, `middle_x2`, `middle_y2` (number, optional): Parameters for 9-slice scaling.
- **Example**:
  ```lua
  {
    type = "background9",
    x = 0, y = 0, w = 5, h = 5,
    texture_name = "bg9.png",
    middle_x = 1, middle_y = 1, middle_x2 = 3, middle_y2 = 3
  }
  ```

### 5. `bgcolor`
- **Usage**: Sets the background color of the formspec.
- **Parameters**:
  - `bgcolor` (string): Color specification.
  - `fullscreen` (fullscreen, optional): Determines fullscreen mode.
  - `fbgcolor` (string, optional): Fullscreen background color.
- **Example**:
  ```lua
  { type = "bgcolor", bgcolor = "#FFF", fullscreen = true, fbgcolor = "#000" }
  ```

### 6. `box`
- **Usage**: Draws a simple colored box.
- **Parameters**:
  - `x`, `y` (number): Position of the box.
  - `w`, `h` (number): Width and height of the box.
  - `color` (string): Color of the box.
- **Example**:
  ```lua
  { type = "box", x = 1, y = 1, w = 3, h = 3, color = "#F00" }
  ```

### 7. `button`
- **Usage**: Creates a clickable button.
- **Parameters**:
  - `x`, `y` (number): Position of the button.
  - `w`, `h` (number): Width and height of the button.
  - `name` (string): Element name for event handling.
  - `label` (string): Text displayed on the button.
- **Example**:
  ```lua
  { type = "button", x = 2, y = 2, w = 4, h = 1, name = "btn1", label = "Click me" }
  ```

### 8. `button_exit`
- **Usage**: Creates a button that, when clicked, will send fields and close the formspec.
- **Parameters**:
  - `x`, `y` (number): Position of the button.
  - `w`, `h` (number): Width and height of the button.
  - `name` (string): Element name for event handling.
  - `label` (string): Text displayed on the button.
- **Example**:
  ```lua
  { type = "button_exit", x = 2, y = 3, w = 4, h = 1, name = "exit_btn", label = "Exit" }
  ```

### 9. `checkbox`
- **Usage**: Creates a checkbox element.
- **Parameters**:
  - `x`, `y` (number): Position of the checkbox.
  - `name` (string): Element name for event handling.
  - `label` (string): Text displayed next to the checkbox.
  - `selected` (boolean, optional): Whether the checkbox is initially selected.
- **Example**:
  ```lua
  { type = "checkbox", x = 1, y = 1, name = "chkbox", label = "Check me", selected = false }
  ```

### 10. `container`
- **Usage**: Starts a container block, moving all contained elements by the specified offset.
- **Parameters**:
  - `x`, `y` (number): Offset for the container.
- **Example**:
  ```lua
  { type = "container", x = 1, y = 1 }
  ```

### 11. `container_end`
- **Usage**: Ends a container block.
- **Parameters**: None.
- **Example**:
  ```lua
  { type = "container_end" }
  ```

### 12. `dropdown`
- **Usage**: Creates a dropdown list.
- **Parameters**:
  - `x`, `y` (number): Position of the dropdown.
  - `w`, `h` (number): Width and height of the dropdown.
  - `name` (string): Element name for event handling.
  - `items` (array of strings): List of items in the dropdown.
  - `selected_idx` (number): Index of the initially selected item.
  - `index_event` (boolean, optional): Determines if the event returns the index of the selected item.
- **Example**:
  ```lua
  {
    type = "dropdown",
    x = 1, y = 2, w = 3, h = 1,
    name = "dropdown1",
    items = {"Option 1", "Option 2", "Option 3"},
    selected_idx = 1
  }
  ```

### 13. `field`
- **Usage**: Creates a textual input field.
- **Parameters**:
  - `x`, `y` (number): Position of the field.
  - `w`, `h` (number): Width and height of the field.
  - `name` (string): Element name for event handling.
  - `label` (string): Text label displayed above the field.
  - `default` (string): Default text in the field.
- **Example**:
  ```lua
  { type = "field", x = 1, y = 1, w = 3, h = 1, name = "field1", label = "Enter text", default = "Hello" }
  ```

### 14. `field_close_on_enter`
- **Usage**: Determines if a field should close the formspec when Enter is pressed.
- **Parameters**:
  - `name` (string): Name of the field.
  - `close_on_enter` (boolean): Whether to close the formspec on Enter.
- **Example**:
  ```lua
  { type = "field_close_on_enter", name = "field1", close_on_enter = false }
  ```

### 15. `field_enter_after_edit`
- **Usage**: For Android clients, specifies if pressing "Done" simulates an Enter keypress.
- **Parameters**:
  - `name` (string): Name of the field.
  - `enter_after_edit` (boolean): Whether "Done" acts as Enter.
- **Example**:
  ```lua
  { type = "field_enter_after_edit", name = "field1", enter_after_edit = true }
  ```

### 16. `formspec_version`
- **Usage**: Sets the formspec version.
- **Parameters**:
  - `version` (number): The version number of the formspec.
- **Example**:
  ```lua
  { type = "formspec_version", version = 3 }
  ```

### 17. `hypertext`
- **Usage**: Displays formatted text with hyperlinks.
- **Parameters**:
  - `x`, `y` (number): Position of the hypertext element.
  - `w`, `h` (number): Width and height of the element.
  - `name` (string): Element name for event handling.
  - `text` (string): Formatted text content.
- **Example**:
  ```lua
  {
    type = "hypertext",
    x = 1, y = 1, w = 5, h = 5,
    name = "info_text",
    text = "<b>Bold Text</b> with <action name='link'>hyperlink</action>"
  }
  ```

### 18. `image`
- **Usage**: Displays an image.
- **Parameters**:
  - `x`, `y` (number): Position of the image.
  - `w`, `h` (number): Width and height of the image.
  - `texture_name` (string): Image resource name.
- **Example**:
  ```lua
  { type = "image", x = 2, y = 2, w = 3, h = 3, texture_name = "example.png" }
  ```

### 19. `image_button`
- **Usage**: Creates a button with an image.
- **Parameters**:
  - `x`, `y` (number): Position of the button.
  - `w`, `h` (number): Width and height of the button.
  - `texture_name` (string): Image displayed on the button.
  - `name` (string): Element name for event handling.
  - `label` (string): Text displayed on the button.
  - `noclip` (boolean, optional): If true, allows the element to exceed formspec bounds.
  - `drawborder` (boolean, optional): If true, draws a border around the button.
  - `pressed_texture_name` (string, optional): Image displayed when the button is pressed.
- **Example**:
  ```lua
  {
    type = "image_button",
    x = 1, y = 1, w = 2, h = 1,
    texture_name = "button_normal.png",
    name = "img_btn",
    label = "Click",
    pressed_texture_name = "button_pressed.png"
  }
  ```

### 20. `image_button_exit`
- **Usage**: Creates an image button that closes the formspec when clicked.
- **Parameters**: Same as `image_button`.
- **Example**:
  ```lua
  {
    type = "image_button_exit",
    x = 3, y = 1, w = 2, h = 1,
    texture_name = "exit_button_normal.png",
    name = "exit_btn",
    label = "Exit",
    pressed_texture_name = "exit_button_pressed.png"
  }
  ```

### 21. `item_image`
- **Usage**: Displays an inventory image of a registered item or node.
- **Parameters**:
  - `x`, `y` (number): Position of the item image.
  - `w`, `h` (number): Width and height of the item image.
  - `item_name` (string): Registered name of the item or node.
- **Example**:
  ```lua
  { type = "item_image", x = 2, y = 2, w = 1, h = 1, item_name = "default:stone" }
  ```

### 22. `item_image_button`
- **Usage**: Creates a button with an item image.
- **Parameters**:
  - `x`, `y` (number): Position of the button.
  - `w`, `h` (number): Width and height of the button.
  - `item_name` (string): Registered name of the item or node.
  - `name` (string): Element name for event handling.
  - `label` (string): Text displayed on the button.
- **Example**:
  ```lua
  {
    type = "item_image_button",
    x = 1, y = 1, w = 2, h = 1,
    item_name = "default:apple",
    name = "item_btn",
    label = "Apple"
  }
  ```

### 23. `label`
- **Usage**: Displays a text label.
- **Parameters**:
  - `x`, `y` (number): Position of the label.
  - `label` (string): Text of the label.
- **Example**:
  ```lua
  { type = "label", x = 1, y = 1, label = "This is a label" }
  ```

### 24. `list`
- **Usage**: Displays an inventory list.
- **Parameters**:
  - `inventory_location` (string): Location of the inventory.
  - `list_name` (string): Name of the list in the inventory.
  - `x`, `y` (number): Position of the list.
  - `w`, `h` (number): Width and height of the list (in inventory slots).
  - `starting_item_index` (number, optional): Index of the first item to display.
- **Example**:
  ```lua
  {
    type = "list",
    inventory_location = "current_player",
    list_name = "main",
    x = 0, y = 0, w = 8, h = 4,
    starting_item_index = 0
  }
  ```

### 25. `listcolors`
- **Usage**: Sets the background and border colors for inventory slots.
- **Parameters**:
  - `slot_bg_normal`, `slot_bg_hover`, `slot_border` (string): Color specifications for different states.
  - `tooltip_bgcolor`, `tooltip_fontcolor` (string, optional): Default colors for tooltips.
- **Example**:
  ```lua
  {
    type = "listcolors",
    slot_bg_normal = "#AAAAAA",
    slot_bg_hover = "#FFFFFF",
    slot_border = "#333333",
    tooltip_bgcolor = "#000000",
    tooltip_fontcolor = "#FFFFFF"
  }
  ```

### 26. `listring`
- **Usage**: Creates a ring of inventory lists for shift-clicking items between them.
- **Parameters**:
  - `inventory_location` (string): Location of the inventory.
  - `list_name` (string): Name of the list in the inventory.
- **Example**:
  ```lua
  { type = "listring", inventory_location = "current_player", list_name = "main" }
  ```

Continuing with the documentation for the remaining elements in the `formspec_ast` library:

### 27. `model`
- **Usage**: Displays a 3D model.
- **Parameters**:
  - `x`, `y` (number): Position of the model.
  - `w`, `h` (number): Width and height of the model.
  - `name` (string): Element name for styling.
  - `mesh` (string): Mesh model to use.
  - `textures` (array of strings): Textures for the mesh, separated by commas.
  - `rotation_x`, `rotation_y` (number, optional): Initial rotation of the camera.
  - `continuous` (boolean, optional): Whether the rotation is continuous.
  - `mouse_control` (boolean, optional): Whether the model can be controlled with the mouse.
  - `frame_loop_begin`, `frame_loop_end` (number, optional): Range of animation frames.
  - `animation_speed` (number, optional): Animation speed in FPS.
- **Example**:
  ```lua
  {
    type = "model",
    x = 1, y = 1, w = 3, h = 3,
    name = "model1",
    mesh = "character.b3d",
    textures = {"character.png"},
    rotation_x = 0, rotation_y = 0,
    continuous = true,
    mouse_control = true,
    frame_loop_begin = 0, frame_loop_end = 80,
    animation_speed = 30
  }
  ```

### 28. `no_prepend`
- **Usage**: Disables `player:set_formspec_prepend()` from applying to this formspec.
- **Parameters**: None.
- **Example**:
  ```lua
  { type = "no_prepend" }
  ```

### 29. `padding`
- **Usage**: Defines space padded around the formspec.
- **Parameters**:
  - `x`, `y` (number): Padding values for X and Y axes.
- **Example**:
  ```lua
  { type = "padding", x = 0.05, y = 0.05 }
  ```

### 30. `position`
- **Usage**: Sets the position of the formspec's anchor point on the game window.
- **Parameters**:
  - `x`, `y` (number): X and Y coordinates for the position.
- **Example**:
  ```lua
  { type = "position", x = 0.5, y = 0.5 }
  ```

### 31. `pwdfield`
- **Usage**: Creates a password field.
- **Parameters**:
  - `x`, `y` (number): Position of the field.
  - `w`, `h` (number): Width and height of the field.
  - `name` (string): Element name for event handling.
  - `label` (string): Text label displayed above the field.
- **Example**:
  ```lua
  { type = "pwdfield", x = 1, y = 1, w = 3, h = 1, name = "pwd", label = "Password" }
  ```

### 32. `real_coordinates`
- **Usage**: Enables the new coordinate system for all following elements.
- **Parameters**:
  - `bool` (boolean): Set to true to enable the new coordinate system.
- **Example**:
  ```lua
  { type = "real_coordinates", bool = true }
  ```

### 33. `scroll_container`
- **Usage**: Starts a scrollable container block.
- **Parameters**:
  - `x`, `y` (number): Position of the scroll container.
  - `w`, `h` (number): Width and height of the container.
  - `scrollbar_name` (string): Name of the scrollbar element.
  - `orientation` (string): Orientation of the scrollbar (`vertical` or `horizontal`).
  - `scroll_factor` (number, optional): Factor by which the scrollbar scrolls.
- **Example**:
  ```lua
  {
    type = "scroll_container",
    x = 1, y = 1, w = 5, h = 5,
    scrollbar_name = "scrollbar1",
    orientation = "vertical",
    scroll_factor = 0.1
  }
  ```

### 34. `scroll_container_end`
- **Usage**: Ends a scroll_container.
- **Parameters**: None.
- **Example**:
  ```lua
  { type = "scroll_container_end" }
  ```

### 35. `scrollbar`
- **Usage**: Creates a scrollbar.
- **Parameters**:
  - `x`, `y` (number): Position of the scrollbar.
  - `w`, `h` (number): Width and height of the scrollbar.
  - `orientation` (string): Orientation of the scrollbar (`vertical` or `horizontal`).
  - `name` (string): Element name for event handling.
  - `value` (number): Initial value of the scrollbar (0-1000).
- **Example**:
  ```lua
  {
    type = "scrollbar",
    x = 6, y = 1, w = 0.4, h = 5,
    orientation = "vertical",
    name = "scrollbar1",
    value = 500
  }
  ```

### 36. `scrollbaroptions`
- **Usage**: Sets options for all following `scrollbar[]` elements.
- **Parameters**:
  - `opts` (table): Table of scrollbar options like `min`, `max`, `smallstep`, `largestep`, `thumbsize`, `arrows`.
- **Example**:
  ```lua
  {
    type = "scrollbaroptions",
    opts = {
      min = 0,
      max = 1000,
      smallstep = 10,
      largestep = 100,
      thumbsize = 10,
      arrows = "show"
    }
  }
  ```

### 37. `set_focus`
- **Usage**: Sets the focus to a specified element.
- **Parameters**:
  - `name` (string): Name of the element to focus.
  - `force` (boolean, optional): If true, forces focus for every sent formspec.
- **Example**:
  ```lua
  { type = "set_focus", name = "field1", force = false }
  ```

### 38. `size`
- **Usage**: Defines the size of the formspec.
- **Parameters**:
  - `w`, `h` (number): Width and height of the formspec.
  - `fixed_size` (boolean, optional): If true, sets a fixed size for the formspec.
- **Example**:
  ```lua
  { type = "size", w = 8, h = 9, fixed_size = false }
  ```

### 39. `style`
- **Usage**: Sets the style for specified elements.
- **Parameters**:
  - `selectors` (array of strings): List of element names or types to apply the style.
  - `props` (table): Table of style properties

 and values.
- **Example**:
  ```lua
  {
    type = "style",
    selectors = {"button", "field"},
    props = {
      bgcolor = "#FF0000",
      textcolor = "#FFFFFF"
    }
  }
  ```
  For props, see [Styling Formspecs](styling-formspecs)
  
### 40. `style_type`
- **Usage**: Similar to `style`, but applies styles based on element types.
- **Parameters**: Same as `style`.
- **Example**:
  ```lua
  {
    type = "style_type",
    selectors = {"button", "label"},
    props = {
      bgcolor = "#00FF00",
      textcolor = "#000000"
    }
  }
  ```
  For props, see [Styling Formspecs](styling-formspecs)
  
### 41. `tabheader`
- **Usage**: Creates a tab header.
- **Parameters**:
  - `x`, `y` (number): Position of the tabheader.
  - `w`, `h` (number, optional): Width and height of the tabheader.
  - `name` (string): Element name for event handling.
  - `captions` (array of strings): Captions for each tab.
  - `current_tab` (number): Index of the currently selected tab.
  - `transparent` (boolean, optional): If true, tabs are semi-transparent.
  - `draw_border` (boolean, optional): If true, draws a border at the base of the tabs.
- **Example**:
  ```lua
  {
    type = "tabheader",
    x = 0, y = 0, w = 5, h = 1,
    name = "tab1",
    captions = {"Tab 1", "Tab 2", "Tab 3"},
    current_tab = 1,
    transparent = false,
    draw_border = true
  }
  ```

### 42. `table`
- **Usage**: Displays a table with specified contents.
- **Parameters**:
  - `x`, `y` (number): Position of the table.
  - `w`, `h` (number): Width and height of the table.
  - `name` (string): Element name for event handling.
  - `cells` (array of strings): Cell contents in row-major order.
  - `selected_idx` (number): Index of the row to be selected.
- **Example**:
  ```lua
  {
    type = "table",
    x = 1, y = 1, w = 5, h = 3,
    name = "table1",
    cells = {"Header 1", "Header 2", "Row 1, Col 1", "Row 1, Col 2"},
    selected_idx = 1
  }
  ```

### 43. `tablecolumns`
- **Usage**: Defines the columns for a table.
- **Parameters**:
  - `type` (string): Type of the column (e.g., `text`, `image`).
  - `opts` (table): Options specific to the column type.
- **Example**:
  ```lua
  {
    type = "tablecolumns",
    {"text", opts = {align = "left", width = "3"}},
    {"image", opts = {0 = "default_image.png"}}
  }
  ```

### 44. `tableoptions`
- **Usage**: Sets options for a table.
- **Parameters**:
  - `opts` (table): Table of options like `color`, `background`, `border`, `highlight`, etc.
- **Example**:
  ```lua
  {
    type = "tableoptions",
    opts = {
      color = "#FFFFFF",
      background = "#000000",
      border = true,
      highlight = "#FF0000"
    }
  }
  ```

### 45. `textarea`
- **Usage**: Creates a multi-line text input area.
- **Parameters**:
  - `x`, `y` (number): Position of the textarea.
  - `w`, `h` (number): Width and height of the textarea.
  - `name` (string): Element name for event handling.
  - `label` (string): Text label displayed above the textarea.
  - `default` (string): Default text in the textarea.
- **Example**:
  ```lua
  {
    type = "textarea",
    x = 1, y = 1, w = 5, h = 3,
    name = "textarea1",
    label = "Description",
    default = "Enter text here..."
  }
  ```

### 46. `textlist`
- **Usage**: Creates a scrollable list of text items.
- **Parameters**:
  - `x`, `y` (number): Position of the textlist.
  - `w`, `h` (number): Width and height of the textlist.
  - `name` (string): Element name for event handling.
  - `listelems` (array of strings): List of text elements.
  - `selected_idx` (number, optional): Index of the initially selected item.
  - `transparent` (boolean, optional): If true, draws a transparent background.
- **Example**:
  ```lua
  {
    type = "textlist",
    x = 1, y = 1, w = 3, h = 5,
    name = "textlist1",
    listelems = {"Item 1", "Item 2", "Item 3"},
    selected_idx = 1,
    transparent = false
  }
  ```

### 47. `tooltip`
- **Usage**: Adds a tooltip to an element or area.
- **Parameters**:
  - `x`, `y`, `w`, `h` (number, optional): Position and size of the tooltip area.
  - `gui_element_name` (string, optional): Name of the GUI element for the tooltip.
  - `tooltip_text` (string): Text of the tooltip.
  - `bgcolor`, `fontcolor` (string, optional): Background and font color of the tooltip.
- **Example**:
  ```lua
  {
    type = "tooltip",
    gui_element_name = "btn1",
    tooltip_text = "This is a button",
    bgcolor = "#FFFFFF",
    fontcolor = "#000000"
  }
  ```

### 48. `vertlabel`
- **Usage**: Displays a vertically oriented text label.
- **Parameters**:
  - `x`, `y` (number): Position of the label.
  - `label` (string): Text of the label.
- **Example**:
  ```lua
  { type = "vertlabel", x = 1, y = 1, label = "Vertical Label" }
  ```

## Styling Formspecs

Valid Properties¶

    animated_image
        noclip - boolean, set to true to allow the element to exceed formspec bounds.
    box
        noclip - boolean, set to true to allow the element to exceed formspec bounds.
            Defaults to false in formspec_version version 3 or higher
        Note: colors, bordercolors, and borderwidths accept multiple input types:
            Single value (e.g. #FF0): All corners/borders.
            Two values (e.g. red,#FFAAFF): top-left and bottom-right,top-right and bottom-left/ top and bottom,left and right.
            Four values (e.g. blue,#A0F,green,#FFFA): top-left/top and rotates clockwise.
            These work similarly to CSS borders.
        colors - ColorString. Sets the color(s) of the box corners. Default black.
        bordercolors - ColorString. Sets the color(s) of the borders. Default black.
        borderwidths - Integer. Sets the width(s) of the borders in pixels. If the width is negative, the border will extend inside the box, whereas positive extends outside the box. A width of zero results in no border; this is default.
    button, button_exit, image_button, item_image_button
        alpha - boolean, whether to draw alpha in bgimg. Default true.
        bgcolor - color, sets button tint.
        bgcolor_hovered - color when hovered. Defaults to a lighter bgcolor when not provided.
            This is deprecated, use states instead.
        bgcolor_pressed - color when pressed. Defaults to a darker bgcolor when not provided.
            This is deprecated, use states instead.
        bgimg - standard background image. Defaults to none.
        bgimg_hovered - background image when hovered. Defaults to bgimg when not provided.
            This is deprecated, use states instead.
        bgimg_middle - Makes the bgimg textures render in 9-sliced mode and defines the middle rect. See background9[] documentation for more details. This property also pads the button's content when set.
        bgimg_pressed - background image when pressed. Defaults to bgimg when not provided.
            This is deprecated, use states instead.
        font - Sets font type. This is a comma separated list of options. Valid options:
        Main font type options. These cannot be combined with each other:
            normal: Default font
            mono: Monospaced font
        Font modification options. If used without a main font type, normal is used:
            bold: Makes font bold.
            italic: Makes font italic. Default normal.
        font_size - Sets font size. Default is user-set. Can have multiple values:
        <number>: Sets absolute font size to number.
        +<number>/-<number>: Offsets default font size by number points.
        *<number>: Multiplies default font size by number, similar to CSS em.
        border - boolean, draw border. Set to false to hide the bevelled button pane. Default true.
        content_offset - 2d vector, shifts the position of the button's content without resizing it.
        noclip - boolean, set to true to allow the element to exceed formspec bounds.
        padding - rect, adds space between the edges of the button and the content. This value is relative to bgimg_middle.
        sound - a sound to be played when triggered.
        textcolor - color, default white.
    checkbox
        noclip - boolean, set to true to allow the element to exceed formspec bounds.
        sound - a sound to be played when triggered.
    dropdown
        noclip - boolean, set to true to allow the element to exceed formspec bounds.
        sound - a sound to be played when the entry is changed.
    field, pwdfield, textarea
        border - set to false to hide the textbox background and border. Default true.
        font - Sets font type. See button font property for more information.
        font_size - Sets font size. See button font_size property for more information.
        noclip - boolean, set to true to allow the element to exceed formspec bounds.
        textcolor - color. Default white.
    model
        bgcolor - color, sets background color.
        noclip - boolean, set to true to allow the element to exceed formspec bounds.
            Default to false in formspec_version version 3 or higher
    image
        noclip - boolean, set to true to allow the element to exceed formspec bounds.
            Default to false in formspec_version version 3 or higher
    item_image
        noclip - boolean, set to true to allow the element to exceed formspec bounds. Default to false.
    label, vertlabel
        font - Sets font type. See button font property for more information.
        font_size - Sets font size. See button font_size property for more information.
        noclip - boolean, set to true to allow the element to exceed formspec bounds.
    list
        noclip - boolean, set to true to allow the element to exceed formspec bounds.
        size - 2d vector, sets the size of inventory slots in coordinates.
        spacing - 2d vector, sets the space between inventory slots in coordinates.
    image_button (additional properties)
        fgimg - standard image. Defaults to none.
        fgimg_hovered - image when hovered. Defaults to fgimg when not provided.
            This is deprecated, use states instead.
        fgimg_pressed - image when pressed. Defaults to fgimg when not provided.
            This is deprecated, use states instead.
        fgimg_middle - Makes the fgimg textures render in 9-sliced mode and defines the middle rect. See background9[] documentation for more details.
        NOTE: The parameters of any given image_button will take precedence over fgimg/fgimg_pressed
        sound - a sound to be played when triggered.
    scrollbar
        noclip - boolean, set to true to allow the element to exceed formspec bounds.
    tabheader
        noclip - boolean, set to true to allow the element to exceed formspec bounds.
        sound - a sound to be played when a different tab is selected.
        textcolor - color. Default white.
    table, textlist
        font - Sets font type. See button font property for more information.
        font_size - Sets font size. See button font_size property for more information.
        noclip - boolean, set to true to allow the element to exceed formspec bounds.
