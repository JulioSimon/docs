# Tonga National Portal - Plugin Functionalities

This document provides comprehensive information about all the plugins installed on the Tonga National Portal, including their features, configuration, and usage guidelines.

## Table of Contents

1. [Announcer Pro](#announcer-pro)
2. [Core Framework](#core-framework)
3. [Document Library Lite](#document-library-lite)
4. [Location Weather Pro](#location-weather-pro)
5. [MxChat](#mxchat)
6. [Popup Maker](#popup-maker)
7. [Site Kit by Google](#site-kit-by-google)
8. [UserFeedback Premium](#userfeedback-premium)
9. [WP Mail SMTP](#wp-mail-smtp)
10. [WPML Multilingual CMS](#wpml-multilingual-cms)

---

## Announcer Pro

### Description
Announcer Pro is a powerful WordPress plugin that enables administrators to create and manage announcements, alerts, and notifications across the Tonga National Portal. It provides a flexible system for communicating important information to site visitors.

### Key Features
- **Multiple Announcement Types**: Create banners, popups, floating notifications, and in-content alerts
- **Scheduling**: Set start and end dates for announcements
- **Targeting**: Display announcements to specific user roles or visitor segments
- **Customization**: Full control over colors, typography, and positioning
- **Analytics**: Track views and interactions with announcements

### Configuration
1. **Basic Setup**:
   - Navigate to **Announcer Pro > Settings** in the WordPress admin dashboard
   - Configure global settings including default styles and behavior

2. **Creating an Announcement**:
   - Go to **Announcer Pro > Add New**
   - Enter a title and content for your announcement
   - Select the announcement type (banner, popup, etc.)
   - Configure display options and targeting rules
   - Set scheduling parameters if needed
   - Click "Publish" to activate

3. **Advanced Configuration**:
   - **Custom CSS**: Add custom styling under the "Custom CSS" tab
   - **API Integration**: Use the provided API endpoints to programmatically create or manage announcements

### Usage Examples

#### Emergency Alert Banner
```
Title: Weather Emergency Alert
Content: Due to approaching tropical storm, all government offices will be closed on Friday, May 15.
Type: Top Banner
Color: Red
Display: All users
Duration: May 14-16
```

#### Welcome Message for New Visitors
```
Title: Welcome to the Tonga National Portal
Content: This portal provides access to all government services. Create an account to personalize your experience.
Type: Modal Popup
Trigger: First visit
Display: Non-logged in users
```

### Best Practices
- Keep announcements concise and focused on a single message
- Use appropriate colors to indicate urgency (red for emergencies, yellow for warnings, etc.)
- Schedule end dates for all announcements to prevent outdated information
- Test announcements on different devices before publishing
- Avoid displaying multiple announcements simultaneously

### Troubleshooting
- **Announcements Not Displaying**: Check targeting rules and scheduling parameters
- **Styling Issues**: Verify custom CSS is properly formatted and not conflicting with theme styles
- **Performance Impact**: If site performance decreases, reduce the number of active announcements
- **Mobile Display Problems**: Test and adjust responsive settings for mobile devices

---

## Core Framework

### Description
Core Framework serves as the foundation for the Tonga National Portal, providing essential functionality, structure, and integration capabilities. It establishes the architectural backbone that other plugins and features build upon.

### Key Features
- **Modular Architecture**: Extensible framework for adding new features
- **API Management**: Centralized API handling and authentication
- **User Management**: Enhanced user roles and permissions system
- **Performance Optimization**: Caching and optimization tools
- **Security Features**: Built-in security measures and monitoring
- **Development Tools**: Hooks, filters, and debugging capabilities

### Configuration
1. **System Settings**:
   - Navigate to **Core Framework > System** in the admin dashboard
   - Configure server environment settings, caching options, and performance parameters
   - Set up security policies and access controls

2. **API Configuration**:
   - Go to **Core Framework > API Settings**
   - Configure API endpoints, authentication methods, and rate limiting
   - Set up integration with external services

3. **User Roles and Permissions**:
   - Access **Core Framework > User Management**
   - Define custom user roles specific to the Tonga National Portal
   - Set granular permissions for different administrative functions

### Usage Examples

#### Creating a Custom Module
```php
// Register a new module
add_action('cf_register_modules', function() {
    cf_register_module([
        'id' => 'tourism_services',
        'name' => 'Tourism Services',
        'description' => 'Tourism-related government services',
        'icon' => 'beach-access',
        'permissions' => ['manage_tourism']
    ]);
});

// Add module content
add_action('cf_module_tourism_services', function() {
    // Module content and functionality
});
```

#### Implementing Custom API Endpoint
```php
// Register custom API endpoint
add_action('cf_api_init', function() {
    cf_register_api_endpoint('tourism/locations', [
        'methods' => 'GET',
        'callback' => 'get_tourism_locations',
        'permission_callback' => function() {
            return current_user_can('read_tourism_data');
        }
    ]);
});

function get_tourism_locations() {
    // API logic here
    return cf_api_response($data);
}
```

### Best Practices
- Keep the Core Framework updated to the latest version
- Use the provided hooks and filters rather than modifying core files
- Implement proper caching strategies for optimal performance
- Follow the established coding standards for custom development
- Document all customizations and extensions
- Regularly back up configuration settings

### Troubleshooting
- **Performance Issues**: Check the performance logs at **Core Framework > Diagnostics > Performance**
- **API Errors**: Review the API error logs and test endpoints using the built-in API tester
- **Permission Problems**: Verify user roles and permissions in the User Management section
- **Caching Issues**: Clear all caches through **Core Framework > Maintenance > Cache**
- **Module Conflicts**: Use the Module Conflict Resolution tool under Diagnostics

---

## Document Library Lite

### Description
Document Library Lite provides a comprehensive solution for managing, organizing, and sharing documents on the Tonga National Portal. It enables government departments to publish and categorize official documents while providing citizens with an easy way to search and access them.

### Key Features
- **Document Management**: Upload, organize, and manage documents in a centralized library
- **Categorization**: Create hierarchical categories and tags for documents
- **Search Functionality**: Advanced search with filters for document type, department, date, etc.
- **Access Control**: Set permissions for viewing and downloading documents
- **Version Control**: Track document versions and updates
- **Metadata**: Add and manage custom metadata for documents
- **Download Tracking**: Monitor document downloads and usage statistics

### Configuration
1. **Basic Setup**:
   - Navigate to **Documents > Settings** in the WordPress admin
   - Configure general settings, file types, and maximum file sizes
   - Set up default categories and tags

2. **Document Upload Settings**:
   - Go to **Documents > Upload Settings**
   - Configure upload permissions and approval workflows
   - Set up document validation rules

3. **Search Configuration**:
   - Access **Documents > Search Settings**
   - Configure search indexing options
   - Set up custom search filters

### Usage Examples

#### Uploading Official Documents
1. Navigate to **Documents > Add New**
2. Fill in document details:
   - Title: "2023 National Budget"
   - Category: "Finance"
   - Department: "Ministry of Finance"
   - Description: "Official national budget for fiscal year 2023"
3. Upload the PDF file
4. Add relevant metadata (fiscal year, approval date, etc.)
5. Set appropriate access permissions
6. Click "Publish"

#### Creating a Document Collection
1. Go to **Documents > Collections > Add New**
2. Create a collection called "COVID-19 Health Guidelines"
3. Add relevant documents to the collection
4. Configure display options and access permissions
5. Use the shortcode `[doc_collection id="covid-guidelines"]` to display on a page

### Best Practices
- Use consistent naming conventions for documents
- Create a logical category hierarchy that aligns with government structure
- Add comprehensive metadata to improve searchability
- Regularly audit document permissions to ensure proper access control
- Use PDF format for official documents to maintain formatting
- Implement a document review and approval workflow for sensitive materials
- Regularly back up the document library

### Troubleshooting
- **Upload Failures**: Check file size limits and allowed file types
- **Search Not Finding Documents**: Rebuild the search index in Settings
- **Permission Issues**: Verify user roles and document-specific permissions
- **Slow Document Library**: Consider enabling document caching or optimizing large files
- **Missing Metadata**: Use the Metadata Repair tool under Documents > Tools

---

## Location Weather Pro

### Description
Location Weather Pro integrates real-time weather information into the Tonga National Portal, providing citizens with accurate weather forecasts, alerts, and climate data specific to different regions of Tonga. This plugin is particularly important for a Pacific island nation where weather conditions can significantly impact daily life and safety.

### Key Features
- **Real-time Weather Data**: Current conditions for all regions of Tonga
- **Forecasting**: 7-day weather forecasts with hourly breakdowns
- **Weather Alerts**: Automatic alerts for severe weather conditions
- **Weather Maps**: Interactive maps showing precipitation, temperature, and wind
- **Historical Data**: Access to historical weather patterns and statistics
- **Weather API**: Integration with international weather services
- **Customizable Displays**: Various widgets and shortcodes for displaying weather information

### Configuration
1. **API Setup**:
   - Navigate to **Weather Pro > Settings > API**
   - Enter your weather service API key (default provider: OpenWeatherMap)
   - Configure update frequency and caching settings

2. **Location Settings**:
   - Go to **Weather Pro > Locations**
   - Add and configure locations for different regions of Tonga
   - Set the default location for the portal homepage

3. **Display Options**:
   - Access **Weather Pro > Display Settings**
   - Configure widget styles, units (metric/imperial), and information to display
   - Set up custom CSS if needed

### Usage Examples

#### Adding a Weather Widget to Homepage
1. Go to **Appearance > Widgets**
2. Add the "Weather Pro - Current Conditions" widget to your desired sidebar or widget area
3. Configure the widget settings:
   - Title: "Current Weather in Nuku'alofa"
   - Location: "Nuku'alofa"
   - Display: Temperature, Conditions, Wind, Humidity
   - Style: "Compact"
4. Save the widget settings

#### Creating a Weather Page with Shortcode
```
[weather_pro_forecast location="Vava'u" days="5" show_hourly="true" show_alerts="true"]
```

#### Embedding Weather Alerts
```
[weather_pro_alerts regions="all" priority="high" banner="true"]
```

### Best Practices
- Configure automatic weather alerts for severe conditions
- Display weather information relevant to the most populated areas
- Use weather widgets strategically on pages related to tourism, transportation, and emergency services
- Ensure weather data is updated frequently during storm seasons
- Provide context and safety recommendations alongside severe weather alerts
- Consider bandwidth limitations when configuring weather maps and graphics

### Troubleshooting
- **Weather Data Not Updating**: Check API key validity and connection status
- **Incorrect Location Data**: Verify coordinates for custom locations
- **Missing Weather Alerts**: Ensure alert settings are properly configured
- **Display Issues**: Test different display settings and check for CSS conflicts
- **High API Usage**: Adjust caching settings to reduce API calls

---

## MxChat

### Description
MxChat provides real-time chat and messaging capabilities for the Tonga National Portal, enabling direct communication between citizens and government representatives. It facilitates efficient customer service, support for government services, and internal communication between departments.

### Key Features
- **Live Chat**: Real-time chat functionality for citizen support
- **Ticketing System**: Convert chats to support tickets when needed
- **Department Routing**: Automatically route inquiries to appropriate departments
- **Chat History**: Searchable archives of previous conversations
- **File Sharing**: Secure document sharing within chats
- **Chatbots**: Configurable automated responses for common questions
- **Analytics**: Comprehensive reporting on chat volume, response times, and satisfaction
- **Mobile Support**: Responsive design for mobile devices

### Configuration
1. **General Setup**:
   - Navigate to **MxChat > Settings**
   - Configure operational hours, offline messages, and general behavior
   - Set up department structure and routing rules

2. **Agent Configuration**:
   - Go to **MxChat > Agents**
   - Add government staff as chat agents
   - Assign agents to departments and set availability

3. **Chatbot Setup**:
   - Access **MxChat > Chatbots**
   - Create automated responses for frequently asked questions
   - Configure chatbot behavior and fallback options

### Usage Examples

#### Setting Up a Department Chat
1. Go to **MxChat > Departments > Add New**
2. Configure department settings:
   - Name: "Immigration Services"
   - Description: "Support for visa and immigration inquiries"
   - Operating Hours: Monday-Friday, 8:00 AM - 4:30 PM
   - Offline Message: "Immigration support is currently offline. Please leave a message or return during business hours."
3. Assign agents to the department
4. Create pre-defined responses for common immigration questions

#### Implementing Chat on a Service Page
```
[mxchat department="tax_services" button_text="Chat with Tax Support" theme="blue"]
```

#### Creating a Chatbot for FAQs
1. Navigate to **MxChat > Chatbots > Add New**
2. Name the chatbot "General Information Bot"
3. Add Q&A pairs:
   - Q: "What are the office hours?"
   - A: "Government offices are open Monday to Friday, 8:30 AM to 4:30 PM."
   - Q: "How do I renew my passport?"
   - A: "Passport renewals can be submitted online through the Passport Services section or in person at the Immigration Office."
4. Configure the chatbot to handle initial inquiries before routing to a live agent

### Best Practices
- Train agents on proper communication protocols and response templates
- Set realistic expectations for chat response times
- Use canned responses for common inquiries to improve efficiency
- Regularly review chat transcripts to identify areas for improvement
- Implement clear escalation procedures for complex issues
- Configure appropriate offline messages for after-hours support
- Use chatbots strategically to handle routine questions

### Troubleshooting
- **Connection Issues**: Check server WebSocket configuration
- **Agent Availability Problems**: Verify agent status and scheduling settings
- **Chatbot Not Responding**: Review trigger phrases and response configuration
- **Routing Failures**: Check department assignment and routing rules
- **Performance Issues**: Monitor concurrent chat limits and server resources

---

## Popup Maker

### Description
Popup Maker enables the creation and management of targeted popups on the Tonga National Portal. These popups can be used for important announcements, service notifications, feedback collection, and guiding users through complex processes.

### Key Features
- **Multiple Popup Types**: Modal popups, notification bars, slide-ins, and more
- **Targeting**: Display popups based on user behavior, pages visited, or user roles
- **Triggers**: Set popups to appear on click, time delay, exit intent, or scroll position
- **Animations**: Various entrance and exit animations
- **Responsive Design**: Mobile-friendly popup layouts
- **A/B Testing**: Test different popup designs and content
- **Analytics**: Track popup views, interactions, and conversion rates

### Configuration
1. **General Settings**:
   - Navigate to **Popup Maker > Settings**
   - Configure global settings like z-index, overlay options, and default animations
   - Set up accessibility features

2. **Creating Popups**:
   - Go to **Popup Maker > Create Popup**
   - Design the popup content and appearance
   - Configure display triggers and targeting rules
   - Set cookies for controlling repeat displays

3. **Advanced Options**:
   - Access **Popup Maker > Tools**
   - Import/export popup configurations
   - Manage popup analytics and performance

### Usage Examples

#### Creating an Important Announcement Popup
1. Go to **Popup Maker > Create Popup**
2. Configure popup settings:
   - Title: "Important Service Outage"
   - Content: "The online payment system will be unavailable on Saturday, June 10, from 10 PM to 2 AM for scheduled maintenance."
   - Size: Medium
   - Position: Center
   - Animation: Fade and slide
   - Trigger: Time delay (3 seconds)
   - Display on: All pages
   - Frequency: Once per day (set cookie)
3. Save and activate the popup

#### Service Guide Popup Triggered by Button
1. Create a popup with step-by-step instructions
2. Set the trigger to "Click Open"
3. Add the following button to your page:
```html
<button class="popup-trigger" data-popup-id="service-guide">How to Apply</button>
```

#### Feedback Collection After Service Completion
```
[popup_trigger id="feedback_form" text="Share Your Experience"]
```

### Best Practices
- Use popups sparingly to avoid annoying users
- Ensure all popups have a clearly visible close button
- Make popups responsive for mobile devices
- Set appropriate frequency with cookies to prevent repeated displays
- Use clear, concise content in popups
- Consider accessibility needs when designing popups
- Test popups across different browsers and devices
- Use analytics to optimize popup performance

### Troubleshooting
- **Popups Not Displaying**: Check targeting rules and trigger settings
- **Z-Index Conflicts**: Adjust z-index in the popup settings
- **Mobile Display Issues**: Test and adjust responsive settings
- **Performance Impact**: Minimize the number of active popups and optimize content
- **Cookie Problems**: Clear browser cookies and test popup frequency settings

---

## Site Kit by Google

### Description
Site Kit by Google integrates essential Google services into the Tonga National Portal, providing valuable insights into site performance, search visibility, and user behavior. This plugin connects the portal with Google Search Console, Google Analytics, PageSpeed Insights, and other Google tools.

### Key Features
- **Unified Dashboard**: Single dashboard for all Google services
- **Search Console Integration**: Monitor search performance and queries
- **Analytics Integration**: Track user behavior and site usage
- **PageSpeed Insights**: Measure and improve site performance
- **AdSense Integration**: Manage advertising (if applicable)
- **Tag Manager**: Simplified tag management
- **Zero-coding Setup**: No manual code insertion required
- **Role-based Access**: Control which administrators can access which data

### Configuration
1. **Initial Setup**:
   - Navigate to **Site Kit > Dashboard**
   - Click "Start Setup" and follow the authentication process
   - Grant necessary permissions to connect Google services

2. **Service Configuration**:
   - Go to **Site Kit > Settings**
   - Configure individual services (Analytics, Search Console, etc.)
   - Set up data sharing between services

3. **Dashboard Customization**:
   - Access **Site Kit > Settings > Admin Settings**
   - Configure which metrics appear on the dashboard
   - Set up role permissions for data access

### Usage Examples

#### Monitoring Search Performance
1. Navigate to **Site Kit > Search Console**
2. Review top search queries bringing users to the portal
3. Identify trending searches and content opportunities
4. Monitor click-through rates for important government services
5. Check for any search penalties or issues

#### Analyzing User Behavior
1. Go to **Site Kit > Analytics**
2. Review key metrics:
   - Most visited pages and services
   - User demographics and locations
   - Device usage (desktop vs. mobile)
   - Bounce rates and session duration
3. Create custom reports for specific government departments

#### Improving Site Performance
1. Access **Site Kit > PageSpeed Insights**
2. Run performance tests for key pages
3. Review recommendations for improvement
4. Implement suggested optimizations
5. Monitor performance trends over time

### Best Practices
- Regularly review analytics data to inform content and service improvements
- Set up custom dashboards for different departments and needs
- Configure proper event tracking for important user interactions
- Use search data to optimize content for citizen searches
- Implement recommended performance improvements
- Respect user privacy and comply with data protection regulations
- Schedule regular performance reviews using PageSpeed Insights

### Troubleshooting
- **Connection Issues**: Re-authenticate with Google services
- **Missing Data**: Check date ranges and filtering options
- **Permission Problems**: Verify user roles and access settings
- **Tracking Code Conflicts**: Check for duplicate analytics implementations
- **Performance Recommendations**: Prioritize critical performance issues

---

## UserFeedback Premium

### Description
UserFeedback Premium enables the Tonga National Portal to collect, analyze, and act upon user feedback across all government services and information pages. This plugin helps improve citizen satisfaction by identifying pain points and gathering suggestions for enhancement.

### Key Features
- **Multiple Feedback Types**: Surveys, ratings, NPS scores, and open comments
- **Targeted Feedback**: Display feedback forms based on page, user behavior, or demographics
- **Visual Feedback**: Allow users to highlight specific elements on the page
- **Sentiment Analysis**: Automatically analyze feedback sentiment
- **Reporting**: Comprehensive dashboards and exportable reports
- **Integration**: Connect with analytics to correlate feedback with user behavior
- **Workflow Management**: Assign and track feedback resolution
- **Multilingual Support**: Collect feedback in multiple languages

### Configuration
1. **General Setup**:
   - Navigate to **UserFeedback > Settings**
   - Configure global settings, notification preferences, and data retention policies
   - Set up integration with analytics platforms

2. **Creating Feedback Forms**:
   - Go to **UserFeedback > Forms > Add New**
   - Design feedback forms with appropriate questions and rating scales
   - Configure display rules and targeting

3. **Response Management**:
   - Access **UserFeedback > Responses**
   - Set up categorization and tagging for feedback
   - Configure workflow for addressing feedback

### Usage Examples

#### Creating a Service Satisfaction Survey
1. Go to **UserFeedback > Forms > Add New**
2. Configure form settings:
   - Title: "Tax Filing Service Feedback"
   - Trigger: After form submission on tax filing pages
   - Questions:
     - "How would you rate your experience with the online tax filing service?" (1-5 stars)
     - "Was the process clear and easy to follow?" (Yes/No)
     - "How could we improve this service?" (Open text)
   - Thank You Message: "Thank you for your feedback. We are committed to improving our services."
3. Set display rules to show on all tax-related service pages
4. Activate the form

#### Implementing a Quick Feedback Widget
```
[user_feedback_widget type="quick_rating" service="passport_renewal" position="bottom-right"]
```

#### Creating an NPS Survey for the Portal
1. Create a new NPS-type feedback form
2. Configure to display to returning visitors after 3 page views
3. Include follow-up questions based on score ranges
4. Set to display once per quarter per user

### Best Practices
- Keep feedback forms short and focused
- Use clear, simple language in questions
- Implement feedback forms at critical points in user journeys
- Regularly review and categorize incoming feedback
- Close the feedback loop by implementing changes based on suggestions
- Communicate changes made as a result of user feedback
- Test feedback forms on different devices and browsers
- Use conditional questions to gather more detailed information when needed

### Troubleshooting
- **Low Response Rates**: Adjust timing and placement of feedback requests
- **Confusing Results**: Review question wording and format
- **Form Display Issues**: Check targeting rules and triggers
- **Data Integration Problems**: Verify API connections and data mapping
- **Language Issues**: Ensure proper translation of all feedback elements

---

## WP Mail SMTP

### Description
WP Mail SMTP ensures reliable email delivery for the Tonga National Portal, handling system notifications, user communications, and automated messages. This plugin resolves common WordPress email delivery issues by properly configuring SMTP (Simple Mail Transfer Protocol) settings.

### Key Features
- **Reliable Email Delivery**: Improved deliverability for all system emails
- **Multiple Mailer Options**: Support for various SMTP providers and services
- **Email Logging**: Track and monitor all outgoing emails
- **Email Testing**: Tools to verify proper configuration
- **Failure Notifications**: Alerts for failed email delivery attempts
- **Email Authentication**: SPF, DKIM, and DMARC support
- **Backup Mailers**: Fallback options if primary method fails
- **White Labeling**: Customize email sender information

### Configuration
1. **General Setup**:
   - Navigate to **WP Mail SMTP > Settings**
   - Select your mailer type (SMTP, SendGrid, Gmail, etc.)
   - Configure authentication credentials
   - Set up from email and name

2. **Advanced Options**:
   - Go to **WP Mail SMTP > Advanced**
   - Configure email logging preferences
   - Set up backup mailer options
   - Configure email headers and authentication

3. **Testing Configuration**:
   - Access **WP Mail SMTP > Tools > Email Test**
   - Send a test email to verify configuration
   - Review delivery reports and logs

### Usage Examples

#### Configuring Government SMTP Server
1. Navigate to **WP Mail SMTP > Settings**
2. Select "Other SMTP" as the mailer
3. Enter configuration details:
   - SMTP Host: `smtp.gov.to`
   - Encryption: TLS
   - Port: 587
   - Authentication: On
   - Username: `notifications@portal.gov.to`
   - Password: [Secure SMTP Password]
4. Set From Email as `notifications@portal.gov.to`
5. Set From Name as "Tonga National Portal"
6. Save settings and run a test email

#### Setting Up Email Logging
1. Go to **WP Mail SMTP > Email Log**
2. Enable email logging
3. Configure retention period (e.g., 30 days)
4. Set up failed email notifications to administrator
5. Configure what email content should be logged

#### Implementing a Backup Mailer
1. Configure primary SMTP settings
2. Go to **WP Mail SMTP > Advanced > Backup Mailer**
3. Enable backup mailer option
4. Configure a different mail service (e.g., SendGrid) as backup
5. Set conditions for when to use the backup mailer

### Best Practices
- Use a dedicated email address for system notifications
- Implement proper SPF, DKIM, and DMARC records for the sending domain
- Regularly monitor email logs for delivery issues
- Set up alerts for failed email deliveries
- Test email delivery after WordPress updates
- Use secure authentication for SMTP connections
- Implement email throttling for bulk emails
- Regularly rotate SMTP passwords for security

### Troubleshooting
- **Emails Not Sending**: Verify SMTP credentials and server settings
- **Authentication Failures**: Check username/password and security settings
- **Emails Going to Spam**: Implement proper email authentication (SPF, DKIM)
- **Delayed Delivery**: Check for rate limiting on your SMTP server
- **Connection Timeouts**: Verify firewall settings and SMTP server availability

---

## WPML Multilingual CMS

### Description
WPML Multilingual CMS enables the Tonga National Portal to be fully multilingual, supporting both Tongan and English languages. This comprehensive solution handles translation of content, navigation, forms, and custom elements throughout the portal.

### Key Features
- **Content Translation**: Translate pages, posts, custom post types, and taxonomies
- **String Translation**: Translate theme and plugin strings
- **Media Translation**: Handle multilingual media attachments
- **Navigation Management**: Create language-specific menus
- **Language Switcher**: Customizable language selection widget
- **SEO Support**: Multilingual SEO optimization
- **Translation Management**: Workflow for managing translation projects
- **Compatibility**: Works with major plugins and themes

### Configuration
1. **Basic Setup**:
   - Navigate to **WPML > Languages**
   - Add and configure languages (Tongan and English)
   - Set the default language and URL format
   - Configure language switcher appearance

2. **Translation Settings**:
   - Go to **WPML > Translation Management**
   - Configure which content types should be translatable
   - Set up translation roles and workflows
   - Configure automatic content synchronization

3. **String Translation**:
   - Access **WPML > String Translation**
   - Scan themes and plugins for translatable strings
   - Translate interface elements and dynamic content

### Usage Examples

#### Setting Up Basic Language Configuration
1. Navigate to **WPML > Languages**
2. Configure languages:
   - Primary Language: English
   - Additional Language: Tongan
   - URL Format: Different languages in directories (e.g., `/to/` for Tongan)
   - Language Switcher: Flag and native language name
3. Save settings

#### Translating a Government Service Page
1. Create the page in the primary language (English)
2. Go to **WPML > Translation Management > Translation Dashboard**
3. Select the page for translation
4. Choose "Send to translation"
5. Assign to a translator or translate directly
6. Complete the translation with appropriate Tongan content
7. Publish the translated page

#### Adding a Language Switcher to the Header
```php
// In header.php or via a hook
<?php do_action('wpml_add_language_selector'); ?>
```

Or using a widget:
1. Go to **Appearance > Widgets**
2. Add the "Language Switcher" widget to your header widget area
3. Configure display options (flags, language names, etc.)

### Best Practices
- Maintain consistent terminology across translations using the WPML glossary
- Create a translation workflow that includes review by native speakers
- Translate meta information (SEO titles, descriptions) for each language
- Use language-specific media when appropriate (e.g., images with text)
- Keep the language switcher visible and accessible on all pages
- Regularly update translations when original content changes
- Test the site thoroughly in all supported languages
- Consider cultural differences when translating content

### Troubleshooting
- **Missing Translations**: Check if content is set as translatable in WPML settings
- **Broken Layouts in Translation**: Verify theme compatibility with WPML
- **String Translation Not Working**: Rescan theme and plugins for strings
- **SEO Issues**: Ensure proper hreflang tags are being generated
- **Performance Issues**: Enable WPML's caching features
- **Media Problems**: Check media translation settings and attachments

---

## Conclusion

The plugins installed on the Tonga National Portal work together to create a comprehensive, user-friendly, and efficient government portal. Regular maintenance, updates, and configuration reviews are essential to ensure optimal performance and security.

For technical support or additional configuration assistance, please contact the portal administration team or refer to the individual plugin documentation.