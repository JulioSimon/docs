# Template Management with Bricks Builder

## Definition

Bricks Builder is a powerful WordPress page builder that allows you to create custom templates for your website without writing code. Templates in Bricks Builder are reusable layouts that define how different parts of your website appear, such as headers, footers, single posts, archive pages, and more.

Key template concepts in Bricks Builder:

- **Global Templates**: Apply to your entire website (headers, footers)
- **Content Templates**: Control how specific content types display (posts, pages, custom post types)
- **Archive Templates**: Define the layout for archive pages, category pages, and other collection displays
- **Template Conditions**: Rules that determine where and when templates are applied
- **Template Hierarchy**: The order of priority when multiple templates could apply to the same page

Templates in Bricks Builder use a visual drag-and-drop interface, making it accessible for beginners while offering advanced capabilities for experienced designers.

## Editor UI Guide

The Bricks Builder editor interface is divided into several key areas that work together to provide a comprehensive template building experience.

### Main Editor Components

1. **Top Bar**
   - Save button
   - Responsive mode toggles (desktop, tablet, mobile)
   - Settings button (opens global settings panel)
   - Preview button (view template as it will appear on the frontend)
   - Structure button (shows template structure in outline form)
   - Revisions button (access previous versions of the template)
   - Exit to Dashboard button

   ![Bricks UI top navigation](./../images/tnp/templateManagement1.png)

2. **Left Sidebar**
   - Elements panel: Contains all available building blocks
   - Element Configurator: Shows configurable options for the selected element
   - Settings: Access to Theme Styles and Page Settings

    ![Left sidebar showing the elements panel](./../images/tnp/templateManagement2.png)

3. **Canvas Area**
   - Main working area where you build your template
   - WYSIWYG (What You See Is What You Get) interface
   - Direct editing of content and styling
   - Context-sensitive controls appear when elements are selected

   ![Canvas area with a partially built template](./../images/tnp/templateManagement3.png)

4. **Right Sidebar**
   - Navigation area for the different page elements
   - Page elements are nestable
   - Posibility to copy and pages elements between pages

   [Screenshot Recommended: Right sidebar showing style options for a selected element]
   ![Structure navigation](./../images/tnp/templateManagement4.png)

### Working with Elements

1. **Adding Elements**
   - Drag elements from the left sidebar onto the canvas
   - Use the "+" button that appears when hovering over containers
   - Search for elements using the search bar at the top of the elements panel

2. **Selecting Elements**
   - Click directly on an element in the canvas
   - Use the Structure panel to select nested elements
   - Use breadcrumb navigation at the bottom of the screen

3. **Moving Elements**
   - Drag and drop elements within the canvas
   - Use the Structure panel to drag elements to new positions
   - Cut and paste elements using keyboard shortcuts or right-click menu

4. **Styling Elements**
   - Use the Style tab in the left sidebar
   - Apply global styles or create custom styles
   - Use responsive controls to adjust styling for different devices

## Manage Templates

Managing templates effectively is crucial for maintaining a well-organized website. Bricks Builder provides comprehensive tools for creating, organizing, and applying templates.

### Creating Templates

1. Navigate to Bricks > Templates in your WordPress admin dashboard
2. Click "Add New" to create a new template
3. Enter a descriptive name for your template
4. Select the template type:
   - Header
   - Footer
   - Single (for individual posts/pages)
   - Section
   - Popup
   - Archive (for collection pages)
   - Search Results
   - Error 404
5. Click "Create Template" to enter the Bricks Builder editor

![New template creation screen with template type options](./../images/tnp/templateManagement5.png)

### Template Conditions

Template conditions determine where your templates will be displayed on your website:

1. In the template editor, click "Settings" in the top bar
2. Navigate to "Theme Styles" -> "Conditions" tab
3. Add one or more conditions:
   - Include: Pages where the template WILL be displayed
   - Exclude: Pages where the template WILL NOT be displayed
4. Available condition types:
   - Entire Website
   - Front page
   - Post page
   - Archive
   - Search results
   - Error page
   - Terms
   - Individual
5. Save your conditions

![Template conditions interface showing include/exclude options](./../images/tnp/templateManagement6.png)

### Template Organization

- **Naming Convention**: Use clear, descriptive names (e.g., "Product Single Template" rather than "Template 1")
- **Tags and Categories**: Assign tags and categories to templates for easier filtering
- **Template Library**: Access your saved templates from the Templates screen
- **Import/Export**: Share templates between sites using the import/export feature

### Template Priority

When multiple templates could apply to the same page, Bricks Builder uses a priority system:

1. Templates with more specific conditions take precedence over general ones
2. Custom templates override default templates
3. User-created templates override theme templates
4. Manual priority adjustment is available in template settings

### Template Revisions

Bricks Builder maintains a history of changes to your templates:

1. Click the "Revisions" button in the top bar while editing a template
2. View a list of previous versions with timestamps and author information
3. Preview any revision before restoring
4. Restore a previous version if needed
5. Compare revisions to see what changed

![Revisions panel showing version history](./../images/tnp/templateManagement7.png)

## Bricks Settings

Bricks Builder includes comprehensive settings that control how templates function and appear across your website.

### Global Settings

Access global settings by clicking the "Settings" icon in the top bar of the Bricks editor:

1. **General**
   - Posts types to edit with Bricks
   - Convert Gutenberg data into Bricsk and viceversa
   - Custom breakpoints
   - Other CSS global configurations

2. **Builder access**
   - Set builder access per user role

3. **Templates**
   - Automatic screenshots on edit
   - Thumbnail dimension options
   - Password protection
   - Loading templates from a remote Bricks installation

4. **Builder**
   - Autosave interval
   - Builder template
   - Canvas configurations
   - Dynamic data configurations

5. **Performance**
   - Disable options for performance
   - Cache query configuration
   - CSS loading methods

6. **Maintenance mode**
   - Toogle to activate and disable the maintenance mode
   - Template selection for maintenance
   - Baypass maintenance options

7. **API Keys**
   - API Keys for several 3rd party software

8. **Custom code**
   - Code exectuion security
   - Custom CSS code
   - Custom scripts loading

![Global settings panel showing the different tabs](./../images/tnp/templateManagement8.png)

### Template-Specific Settings

Each template has its own settings that override global settings:

1. **Layout Settings**
   - Template width and maximum width
   - Content area padding
   - Background settings

2. **Typography Settings**
   - Font family for headings and body text
   - Font sizes and line heights
   - Text colors and styles

3. **Advanced Settings**
   - Custom CSS classes
   - Custom attributes
   - Template visibility conditions

### Responsive Settings

Bricks Builder provides tools to ensure your templates look great on all devices:

1. **Breakpoints**
   - Define custom breakpoints for different screen sizes
   - Default breakpoints: Desktop, Tablet, Mobile
   - Preview your template at different breakpoints

2. **Responsive Controls**
   - Hide/show elements based on screen size
   - Adjust spacing, sizing, and alignment for each breakpoint
   - Set different styles for different devices

3. **Responsive Images**
   - Configure different image sizes for different devices
   - Set aspect ratios that maintain across screen sizes
   - Enable/disable lazy loading for images

## Custom Fonts

Bricks Builder allows you to use custom fonts throughout your templates, giving you complete typographic control over your website's appearance.

### Adding Custom Fonts

1. Navigate to Bricks > Settings in your WordPress admin dashboard
2. Select the "Custom Fonts" tab
3. Click "Add Custom Font"
4. Enter the font details:
   - Font name (how it will appear in the font selector)
   - Font files (upload or link to font files)
   - Font weight (regular, bold, light, etc.)
   - Font style (normal, italic)
5. Save your changes

[Screenshot Recommended: Custom font addition interface]

### Font Types Supported

1. **Local Fonts**
   - Upload font files directly to your server
   - Supported formats: WOFF2, WOFF, TTF, EOT, SVG
   - Better performance and privacy compared to external fonts

2. **Google Fonts**
   - Access the entire Google Fonts library
   - Select specific weights and styles to optimize loading
   - Enable/disable Google Fonts in performance settings

3. **Adobe Fonts (Typekit)**
   - Connect your Adobe Fonts account
   - Use your licensed Adobe fonts in templates
   - Enter your Adobe Fonts Project ID in settings

4. **Custom Font Services**
   - Add fonts from other providers using custom CSS
   - Support for services like Font Awesome, Fontspring, etc.

### Using Custom Fonts in Templates

1. Select any text element in your template
2. Open the Style tab in the right sidebar
3. Find the Typography section
4. Select your custom font from the Font Family dropdown
5. Adjust weight, style, size, and other typography settings
6. Apply the font to specific elements or set as a global default

### Font Performance Optimization

- **Subset Selection**: Choose only the character sets you need
- **Font Display Settings**: Control how fonts load and display
- **Preloading**: Configure critical fonts to preload for better performance
- **Local Hosting**: Host Google Fonts locally for improved page speed

## System Information

The System Information section provides valuable data about your Bricks Builder installation and server environment, which is essential for troubleshooting and optimization.

### Accessing System Information

1. Navigate to Bricks > Settings in your WordPress admin dashboard
2. Select the "System Information" tab
3. View comprehensive information about your setup

[Screenshot Recommended: System Information screen showing server details]

### Available Information

1. **WordPress Environment**
   - WordPress version
   - Site URL and home URL
   - WP debug mode status
   - Memory limit
   - Active theme

2. **Server Environment**
   - PHP version and memory limit
   - MySQL/MariaDB version
   - Server software (Apache, Nginx, etc.)
   - SSL status
   - Server architecture

3. **Bricks Builder Information**
   - Bricks version
   - License status
   - Activated features
   - Database tables
   - Template count

4. **Active Plugins**
   - List of active plugins with versions
   - Plugin conflicts or compatibility notes
   - Required plugins status

### Using System Information for Troubleshooting

1. **Performance Issues**
   - Check PHP memory limits
   - Verify server resources are adequate
   - Review active plugins for conflicts

2. **Compatibility Problems**
   - Ensure PHP version meets requirements
   - Check for outdated plugins
   - Verify WordPress version compatibility

3. **Support Requests**
   - Copy system information when submitting support tickets
   - Identify potential issues before contacting support
   - Provide complete environment details for faster resolution

### Maintenance Recommendations

Based on your system information, Bricks Builder may provide recommendations for:

- PHP version upgrades
- Memory limit increases
- Plugin updates or replacements
- Server configuration changes
- Performance optimizations

Regular review of your system information helps ensure optimal performance and compatibility as your website grows and evolves.
