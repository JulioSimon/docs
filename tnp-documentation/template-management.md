# Template Management with Bricks Builder

This document provides comprehensive information about using the Bricks template builder for managing templates in the Tonga National Portal. It covers the template system architecture, creation and editing processes, template hierarchy, and best practices for template management.

## Table of Contents

1. [Introduction to Bricks Template Builder](#introduction-to-bricks-template-builder)
2. [Template System Architecture](#template-system-architecture)
3. [Creating and Editing Templates](#creating-and-editing-templates)
4. [Template Hierarchy](#template-hierarchy)
5. [Managing Specific Template Types](#managing-specific-template-types)
6. [Best Practices for Template Design](#best-practices-for-template-design)
7. [Performance Optimization](#performance-optimization)
8. [Template Backup and Restoration](#template-backup-and-restoration)
9. [Troubleshooting Common Issues](#troubleshooting-common-issues)

---

## Introduction to Bricks Template Builder

### Description

Bricks Template Builder is a powerful visual page builder integrated into the Tonga National Portal that enables administrators and content managers to create and manage website templates without coding knowledge. It provides a flexible system for designing consistent layouts across the portal while maintaining the government's visual identity.

### Key Features

- **Visual Editing**: WYSIWYG interface for creating and editing templates
- **Drag-and-Drop Interface**: Easily position and arrange elements
- **Responsive Design Controls**: Create templates that work across all devices
- **Global Elements**: Reusable components across multiple templates
- **Dynamic Data Integration**: Connect templates with WordPress data sources
- **Custom CSS/JS**: Advanced styling and functionality options
- **Template Conditions**: Control where templates are applied
- **Performance Optimization**: Built-in tools for fast-loading templates

### Role in the Tonga National Portal

Bricks Template Builder serves as the foundation for the portal's visual presentation and user experience by:

- Ensuring consistent branding across all government departments
- Providing standardized layouts for different content types
- Enabling rapid deployment of new sections and features
- Supporting multilingual content presentation
- Maintaining accessibility standards for all citizens
- Facilitating responsive design for mobile users

![Screenshot: Bricks Template Builder Interface]
(Here a screenshot would show the Bricks Template Builder interface with the Tonga National Portal template being edited)

---

## Template System Architecture

### Template Types

The Tonga National Portal template system includes these primary template types:

1. **Global Templates**: Applied site-wide (headers, footers, popups)
2. **Content Templates**: Applied to specific content types (pages, posts, custom post types)
3. **Archive Templates**: Applied to content listings (news archives, department listings)
4. **Single Templates**: Applied to individual content items (single news article, service page)

### Template Components

Each template consists of these components:

- **Sections**: Major horizontal divisions of the layout
- **Containers**: Content wrappers within sections
- **Elements**: Individual content blocks (text, images, buttons, etc.)
- **Global Elements**: Reusable components shared across templates
- **Dynamic Data Fields**: Content placeholders populated from the database

### Template Storage and Management

Templates are stored and managed through:

- **WordPress Database**: Core template data and settings
- **Template Library**: Collection of pre-designed templates
- **Export/Import System**: Backup and transfer capabilities
- **Version Control**: History of template changes

### Integration with WordPress

Bricks templates integrate with WordPress through:

- **Template Hierarchy Override**: Replacing default WordPress templates
- **Custom Post Type Integration**: Templates for government-specific content types
- **Dynamic Data Sources**: Connection to WordPress content and metadata
- **Hook System**: Integration points for custom functionality
- **Multilingual Support**: WPML integration for template translation

![Diagram: Template System Architecture]
(Here a diagram would show the relationship between template types, WordPress, and the front-end display)

---

## Creating and Editing Templates

### Accessing the Template Builder

1. Navigate to **Bricks > Templates** in the WordPress admin dashboard
2. Click **Add New** to create a new template or select an existing template to edit
3. Enter a descriptive name for your template
4. Select the template type (global, content, archive, or single)
5. Click **Edit with Bricks** to open the visual editor

### Template Builder Interface

The Bricks Template Builder interface consists of:

- **Canvas**: Central area where you build and preview the template
- **Structure Panel**: Hierarchical view of all elements in the template
- **Elements Panel**: Library of available elements to add to the template
- **Settings Panel**: Configuration options for the selected element
- **Top Bar**: Global controls, responsive preview, and save options

### Creating a Basic Template

1. **Start with Structure**:
   - Add sections to create the main layout divisions
   - Add containers within sections to control content width
   - Configure responsive behavior for different screen sizes

2. **Add Content Elements**:
   - Drag elements from the Elements Panel to your layout
   - Common elements include headings, text, images, buttons, and dividers
   - Configure each element's settings in the Settings Panel

3. **Style Your Template**:
   - Set colors, typography, spacing, and borders
   - Apply the Tonga government brand guidelines
   - Ensure consistent styling across elements

4. **Add Dynamic Data**:
   - Insert dynamic fields where content should be pulled from WordPress
   - Configure dynamic data sources and fallback content
   - Test with different content scenarios

5. **Set Template Conditions**:
   - Define where the template should be applied
   - Configure display rules based on content type, categories, or other criteria
   - Set template priority if multiple templates could apply

### Saving and Publishing Templates

1. Click the **Save** button in the top bar to save your changes
2. Review the template in the preview mode
3. Configure template settings and conditions
4. Publish the template to make it live on the site

![Screenshot: Creating a new template]
(Here a screenshot would show the process of creating a new template with sections and elements)

---

## Template Hierarchy

### Understanding Template Hierarchy

The template hierarchy determines which template is applied to different parts of the website. The Tonga National Portal uses a combination of WordPress's native template hierarchy and Bricks' template conditions system.

### Hierarchy Levels (from highest to lowest priority)

1. **Specific Single Templates**: Templates targeting specific content items by ID
2. **Custom Post Type Single Templates**: Templates for specific custom post types
3. **Category/Taxonomy Templates**: Templates for specific taxonomies
4. **Archive Templates**: Templates for content listings
5. **Default Single Templates**: General templates for content types
6. **Global Templates**: Site-wide templates like header and footer
7. **WordPress Default Templates**: Native WordPress fallback templates

### Template Conditions

Template conditions allow you to specify exactly where each template should be applied:

- **Content Type**: Apply to specific post types (pages, posts, services, departments)
- **Taxonomy**: Apply to specific categories, tags, or custom taxonomies
- **Post/Page**: Apply to specific individual content items
- **User Role**: Show different templates based on user roles
- **Language**: Display different templates based on selected language

### Conflict Resolution

When multiple templates could apply to the same content:

1. Templates with more specific conditions take precedence
2. Templates with higher priority settings override lower priority templates
3. Recently published templates override older templates with the same conditions
4. Manual priority can be set in template settings

![Diagram: Template Hierarchy Flowchart]
(Here a diagram would show the decision flow for template selection)

---

## Managing Specific Template Types

### Header Templates

Headers are global templates that appear at the top of every page.

#### Creating a Header Template

1. Go to **Bricks > Templates > Add New**
2. Name your template (e.g., "Main Header")
3. Select **Header** as the template type
4. Click **Edit with Bricks**
5. Build your header with these common elements:
   - Logo (using the Image element)
   - Navigation Menu (using the Nav Menu element)
   - Search Bar (using the Search element)
   - Language Switcher (using the WPML element)
   - Call-to-Action buttons

#### Header Template Best Practices

- Keep headers clean and focused on navigation
- Ensure the government logo is prominently displayed
- Make the navigation intuitive and accessible
- Include a mobile-responsive menu for smaller screens
- Incorporate the language switcher in a visible location
- Consider a sticky header for better user experience

### Footer Templates

Footers are global templates that appear at the bottom of every page.

#### Creating a Footer Template

1. Go to **Bricks > Templates > Add New**
2. Name your template (e.g., "Main Footer")
3. Select **Footer** as the template type
4. Click **Edit with Bricks**
5. Build your footer with these common elements:
   - Government seal/logo
   - Copyright information
   - Contact details
   - Quick links to important pages
   - Social media links
   - Privacy and terms links

#### Footer Template Best Practices

- Include all required legal information
- Organize content in clear columns or sections
- Provide easy access to important resources
- Ensure contact information is up-to-date
- Include a back-to-top button for user convenience
- Maintain consistent branding with the rest of the site

### Page Templates

Page templates are applied to standard WordPress pages.

#### Creating a Page Template

1. Go to **Bricks > Templates > Add New**
2. Name your template (e.g., "Standard Page")
3. Select **Single** as the template type
4. Set condition to apply to **Pages**
5. Click **Edit with Bricks**
6. Build your page template with:
   - Hero section with page title
   - Content area for the main page content
   - Sidebar for related information (if needed)
   - Call-to-action sections

#### Page Template Variations

- **Homepage Template**: Special layout for the portal homepage
- **Contact Page Template**: Including contact forms and maps
- **Service Page Template**: Structured layout for government services
- **Department Page Template**: Template for ministry/department pages
- **Landing Page Template**: For campaign or special initiative pages

### Archive Templates

Archive templates display collections of content items.

#### Creating an Archive Template

1. Go to **Bricks > Templates > Add New**
2. Name your template (e.g., "News Archive")
3. Select **Archive** as the template type
4. Set condition to apply to specific post type archives
5. Click **Edit with Bricks**
6. Build your archive template with:
   - Archive title and description
   - Filtering options
   - Query Loop element to display posts
   - Pagination controls

#### Archive Template Best Practices

- Provide clear filtering and sorting options
- Display content in a consistent grid or list format
- Include pagination for large content collections
- Show relevant metadata (date, category, author)
- Optimize for both desktop and mobile viewing
- Include search functionality within the archive

### Single Post Templates

Single post templates display individual blog posts or news articles.

#### Creating a Single Post Template

1. Go to **Bricks > Templates > Add New**
2. Name your template (e.g., "News Article")
3. Select **Single** as the template type
4. Set condition to apply to **Posts**
5. Click **Edit with Bricks**
6. Build your single post template with:
   - Featured image display
   - Post title and metadata (date, author, categories)
   - Content area for the post body
   - Social sharing buttons
   - Related posts section
   - Comments section (if enabled)

#### Single Post Template Best Practices

- Prioritize readability with appropriate typography
- Include clear publication date and author information
- Provide easy navigation to related content
- Include social sharing options for important announcements
- Ensure proper display of embedded media (images, videos)
- Consider print-friendly styling

### Custom Post Type Templates

Custom post type templates are designed for government-specific content types.

#### Creating a Custom Post Type Template

1. Go to **Bricks > Templates > Add New**
2. Name your template (e.g., "Service Detail")
3. Select **Single** as the template type
4. Set condition to apply to your custom post type
5. Click **Edit with Bricks**
6. Build your template with fields specific to that content type

#### Common Government Custom Post Types

- **Services**: Government service details
- **Departments**: Ministry and department information
- **Officials**: Government official profiles
- **Documents**: Official document presentations
- **Events**: Government events and ceremonies
- **Projects**: Government initiatives and projects

![Screenshot: Template Conditions Interface]
(Here a screenshot would show setting template conditions for different content types)

---

## Best Practices for Template Design

### Visual Consistency

- **Maintain Brand Guidelines**: Follow the Tonga government brand guidelines for colors, typography, and visual elements
- **Consistent Spacing**: Use standardized spacing between elements
- **Element Styling**: Apply consistent styling to buttons, forms, and interactive elements
- **Typography Hierarchy**: Establish clear heading and text styles
- **Color Usage**: Use the official color palette consistently

### Accessibility Considerations

- **Color Contrast**: Ensure text has sufficient contrast against backgrounds
- **Text Sizing**: Use relative units for text to support user scaling
- **Keyboard Navigation**: Make sure all interactive elements are keyboard accessible
- **Screen Reader Support**: Add appropriate ARIA labels and alt text
- **Focus Indicators**: Provide visible focus states for interactive elements
- **Form Labels**: Ensure all form fields have proper labels

### Responsive Design

- **Mobile-First Approach**: Design for mobile devices first, then enhance for larger screens
- **Flexible Layouts**: Use percentage-based widths and flexible grids
- **Breakpoint Strategy**: Define consistent breakpoints for responsive adjustments
- **Touch Targets**: Ensure buttons and links are large enough for touch interaction
- **Content Prioritization**: Adjust content display priority on smaller screens
- **Testing**: Test templates on various devices and screen sizes

### Performance Considerations

- **Image Optimization**: Compress and properly size images
- **Minimal Animation**: Use animations sparingly and efficiently
- **Element Efficiency**: Avoid unnecessary nested elements
- **CSS Efficiency**: Minimize custom CSS and leverage Bricks' built-in styling
- **Lazy Loading**: Enable lazy loading for below-the-fold images
- **Font Loading**: Optimize web font loading and provide fallbacks

### Content Structure

- **Clear Hierarchy**: Establish a clear visual hierarchy of information
- **Scannable Content**: Design for easy scanning with headings and lists
- **Consistent Navigation**: Provide consistent navigation patterns
- **Content Grouping**: Group related information logically
- **White Space**: Use appropriate spacing to improve readability
- **Call-to-Action Clarity**: Make primary actions stand out visually

![Example: Responsive Design Demonstration]
(Here an image would show how a template adapts across different screen sizes)

---

## Performance Optimization

### Template Performance Factors

Template performance directly impacts site speed and user experience. Key factors affecting template performance include:

- **Template Complexity**: Number and complexity of elements
- **Asset Loading**: How images, scripts, and styles are loaded
- **Dynamic Data Queries**: Efficiency of database queries
- **Caching Implementation**: How templates utilize caching
- **Code Efficiency**: Quality of generated HTML, CSS, and JavaScript

### Optimization Techniques

#### Reducing Template Complexity

- Minimize the number of elements and nested structures
- Use global elements for repeated components
- Avoid unnecessary divs and wrapper elements
- Combine similar elements when possible
- Use the Structure Panel to identify and clean up complex areas

#### Asset Optimization

- Enable image optimization in Bricks settings
- Use WebP image format when possible
- Implement lazy loading for images
- Minimize custom fonts and icon sets
- Optimize and compress custom CSS and JavaScript

#### Dynamic Data Efficiency

- Limit the number of dynamic data queries per template
- Use transients or object caching for expensive queries
- Implement pagination for large data sets
- Avoid redundant queries across multiple elements
- Use query monitor to identify inefficient queries

#### Caching Implementation

- Enable template caching in Bricks settings
- Configure server-side caching for templates
- Implement browser caching for static assets
- Use CDN for global template assets
- Set appropriate cache invalidation rules

### Performance Testing

- **Google PageSpeed Insights**: Test templates for performance issues
- **GTmetrix**: Analyze template loading and rendering
- **WebPageTest**: Test templates across different devices and connections
- **Chrome DevTools**: Analyze rendering performance and identify bottlenecks
- **Query Monitor**: Track database queries generated by templates

### Performance Monitoring

- Regularly test template performance after updates
- Monitor server response times for template rendering
- Track performance metrics in Google Analytics
- Set up alerts for performance degradation
- Conduct periodic performance audits

![Chart: Performance Comparison]
(Here a chart would show performance metrics before and after optimization)

---

## Template Backup and Restoration

### Backup Methods

#### Built-in Template Export

1. Go to **Bricks > Templates**
2. Select the templates you want to export
3. Click the **Export** button
4. Save the JSON file to your computer

#### Full Site Backup

1. Use a WordPress backup plugin to create complete backups
2. Ensure the backup includes the database and files
3. Schedule regular automated backups
4. Store backups in secure, off-site locations

### Restoration Methods

#### Template Import

1. Go to **Bricks > Templates**
2. Click the **Import** button
3. Select your previously exported JSON file
4. Choose which templates to import
5. Click **Import** to restore the templates

#### Database Restoration

For complete template restoration from a database backup:

1. Restore the WordPress database from backup
2. Verify template functionality after restoration
3. Check for any missing media or assets
4. Test templates across different content types

### Version Control

#### Template Versioning

1. Use descriptive names with version numbers for templates
2. Export templates before making significant changes
3. Document changes made to templates
4. Maintain a library of template versions

#### Collaborative Workflow

When multiple administrators work on templates:

1. Establish a check-out system for template editing
2. Document all changes made to templates
3. Review template changes before publishing
4. Maintain a central repository of approved templates

### Emergency Recovery

In case of template corruption or failure:

1. Deactivate problematic templates
2. Restore from the most recent backup
3. Apply a default template temporarily if needed
4. Diagnose the cause of the failure before reimplementing changes

![Diagram: Backup and Restoration Workflow]
(Here a diagram would show the backup and restoration process)

---

## Troubleshooting Common Issues

### Display Issues

#### Problem: Template Not Applying to Content

**Possible Causes:**
- Template conditions are not correctly configured
- Another template has higher priority
- Template is disabled or in draft status
- Caching is serving old template versions

**Solutions:**
1. Check template conditions in **Bricks > Templates**
2. Review template priority settings
3. Verify template status is set to "Published"
4. Clear all caches (Bricks, WordPress, server, browser)
5. Check for conflicts with other plugins

#### Problem: Responsive Design Issues

**Possible Causes:**
- Missing responsive settings for elements
- Conflicting media queries
- Fixed width elements breaking layouts
- Improper nesting of responsive containers

**Solutions:**
1. Use the responsive preview mode to test all breakpoints
2. Check element settings for each breakpoint
3. Replace fixed widths with percentage or responsive units
4. Simplify complex nested structures
5. Test on actual devices, not just browser preview

### Dynamic Data Issues

#### Problem: Dynamic Content Not Displaying

**Possible Causes:**
- Incorrect dynamic data field selection
- Missing content in the data source
- Permission issues accessing the data
- Query filters excluding the content

**Solutions:**
1. Verify dynamic data field mapping in element settings
2. Check if the source content exists and has data
3. Test with an administrator account to rule out permissions
4. Review query filters and parameters
5. Enable Bricks debug mode to see data sources

#### Problem: Slow Template Loading

**Possible Causes:**
- Inefficient database queries
- Too many dynamic data elements
- Large unoptimized images
- Excessive custom CSS/JS
- Server resource limitations

**Solutions:**
1. Use Query Monitor to identify slow queries
2. Consolidate dynamic data requests
3. Optimize and compress images
4. Minimize and combine custom code
5. Implement caching for dynamic elements

### Editing Issues

#### Problem: Changes Not Saving

**Possible Causes:**
- Browser cache interference
- Insufficient user permissions
- PHP memory limits reached
- Plugin conflicts
- Server timeout during save

**Solutions:**
1. Clear browser cache and cookies
2. Verify user has correct editing permissions
3. Increase PHP memory limit in wp-config.php
4. Temporarily disable other plugins to check for conflicts
5. Increase server timeout limits

#### Problem: Editor Loading Slowly

**Possible Causes:**
- Template has too many elements
- Large media files in the template
- Browser extensions interfering
- Insufficient server resources
- Outdated Bricks version

**Solutions:**
1. Simplify complex templates by using more global elements
2. Optimize media files used in the editor
3. Try with browser extensions disabled
4. Check server resource usage during editing
5. Update Bricks to the latest version

### Integration Issues

#### Problem: WPML Translation Issues

**Possible Causes:**
- Template strings not registered for translation
- Dynamic content not configured for translation
- Language switcher not properly implemented
- Missing translations for template elements

**Solutions:**
1. Register template strings for translation in WPML
2. Configure dynamic data to pull language-specific content
3. Verify language switcher implementation
4. Check for missing translations in WPML String Translation

#### Problem: Plugin Compatibility Issues

**Possible Causes:**
- Plugins modifying the same output areas
- JavaScript conflicts between plugins
- CSS conflicts affecting template styling
- Plugin hooks interfering with template rendering

**Solutions:**
1. Check for plugins that modify the same areas of the site
2. Use browser developer tools to identify JavaScript errors
3. Use CSS specificity to override conflicting styles
4. Consult with plugin developers for compatibility solutions
5. Consider alternative plugins with better compatibility

![Screenshot: Troubleshooting Interface]
(Here a screenshot would show the Bricks debug mode with troubleshooting information)

---

## Conclusion

The Bricks Template Builder provides a powerful and flexible system for managing the Tonga National Portal's visual presentation and user experience. By following the guidelines and best practices outlined in this document, government staff can create and maintain templates that are consistent, accessible, and performant.

For more information on other aspects of the Tonga National Portal, please refer to the following documentation:

- [Platform Structure](./platform-structure.md)
- [Generic Functionalities](./generic-functionalities.md)
- [Plugin Functionalities](./plugins-functionalities.md)