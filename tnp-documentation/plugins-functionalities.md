# Tonga National Portal - Plugin Functionalities

This document provides comprehensive information about all the plugins installed on the Tonga National Portal, including their features, configuration, and usage guidelines.

## Table of Contents

1. [Announcer Pro](#announcer-pro)
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

Announcer Pro is a premium WordPress plugin developed by Aakash Web that enables website administrators to create and display eye-catching notification bars (also called message banners or sticky bars) on their WordPress websites. As a standalone plugin with enhanced capabilities beyond the free version, Announcer Pro provides a comprehensive solution for displaying important announcements, promotional messages, special offers, and time-sensitive information to website visitors in an engaging and non-intrusive manner.

The plugin allows for complete customization of announcement appearance, positioning, and behavior, making it an essential tool for increasing user engagement, driving conversions, and communicating important information effectively across your website.

![Announcer message at Tonga National Portal homepage](./../images/tnp/pf5.png)

### Key Features

- **Multiple Announcement Creation**: Create and manage unlimited announcements with different designs, content, and configurations.
  
- **Flexible Positioning**: Place announcements at the top, bottom, or custom positions on your website with precise control over appearance.
  
- **Multiple Messages Support**: Display multiple messages within a single announcement bar that rotate automatically like a ticker.
  
- **Advanced Targeting Options**: Control announcement visibility based on specific pages, posts, user roles, devices, and custom conditions.
  
- **Visitor Conditions**: Target visitors based on specific criteria
  
- **Countdown Timer**: Add dynamic countdown timers to create urgency for limited-time offers, sales, or events.
  
- **Animation Effects**: Apply attention-grabbing animations to both announcements and call-to-action buttons to increase engagement.
  
- **Scheduling Options**: Set specific date and time ranges for announcements to automatically display and expire.
  
- **Shortcode Support**: Insert announcements anywhere on your site using simple shortcodes, including within posts, pages, and widgets.
  
- **Responsive Design**: Ensure announcements display properly across all devices with mobile-friendly layouts.
  
- **Close Button Customization**: Configure how and when users can dismiss announcements, with options to remember user preferences.
  
- **Analytics Integration**: Track announcement performance with built-in view and click statistics.

### Adding a New Announcement

Creating a new announcement in Announcer Pro involves a straightforward process that allows for extensive customization:

1. **Access the Announcer Dashboard**:
   - Log in to your WordPress admin panel
   - Navigate to "Announcer" in the left sidebar menu
   - Click on "Add New" to create a new announcement
   ![Announcer menu with Add New highlighted](./../images/tnp/pf1.png)

2. **Configure Basic Settings**:
   - Enter a descriptive title for internal reference
   - Add your announcement message in the content editor
     - Use the rich text editor to format text, add links, and insert media
     - Include call-to-action buttons as needed
   - Select the announcement type (notification bar, floating banner, etc.)

3. **Design Your Announcement**:
   - Choose from pre-designed templates or create a custom design
   - Set colors for background, text, and buttons
   - Configure typography settings (font family, size, weight)
   - Add borders, shadows, and other visual elements
   - Adjust padding and spacing for optimal appearance
   ![Announcer predefined layout options](./../images/tnp/pf2.png)

4. **Position the Announcement**:
   - Select placement location (top, bottom, or custom position)
   - Configure display behavior (fixed, static, or slide-in)
   - Set z-index to control stacking with other page elements
   - Adjust responsive behavior for different screen sizes

5. **Save and Publish**:
   - Click "Save Draft" to save your work without publishing
   - Use "Preview" to see how the announcement will appear on your site
   - Click "Publish" to make the announcement live on your website

### Announcement Configuration

After creating a basic announcement, Announcer Pro offers extensive configuration options to fine-tune its behavior and targeting:

1. **Display Conditions**:
   - **Page Targeting**: Select specific pages, posts, categories, or custom post types where the announcement should appear
   - **Exclusion Rules**: Define pages or sections where the announcement should not be shown
   - **Device Targeting**: Choose to display on desktop, mobile, tablet, or specific devices only
   - **User Targeting**: Show announcements based on user roles, login status, or custom user attributes
   ![Show announcements based on user roles, login status, or custom user attributes](./../images/tnp/pf3.png)

2. **Timing Controls**:
   - **Schedule**: Set start and end dates/times for the announcement
   - **Display Frequency**: Control how often the announcement appears to the same visitor
   - **Delay Options**: Add a time delay before showing the announcement
   - **Scroll Trigger**: Display the announcement after the user scrolls to a certain point

3. **Advanced Visitor Conditions**:
   - **Referrer Source**: Show announcements only to visitors coming from specific websites or search engines
   - **Query Parameters**: Target based on URL parameters (useful for campaign tracking)
   - **Cookie-Based Rules**: Display based on the presence or value of specific cookies

4. **Behavior Settings**:
   - **Close Button Options**: Customize appearance and behavior of the close button
   - **Cookie Duration**: Set how long the announcement remains hidden after a user closes it
   - **Animation Settings**: Configure entrance and exit animations
   - **Interaction Triggers**: Define actions that cause the announcement to appear or disappear
   ![Customize appearance and behavior of the close button](./../images/tnp/pf4.png)

5. **Multiple Messages Configuration**:
   - Add multiple content blocks to a single announcement
   - Configure rotation speed and transition effects
   - Set display order (sequential or random)
   - Control navigation options for users to browse messages

6. **Countdown Timer Setup**:
   - Select timer style and appearance
   - Set the target date and time
   - Configure what happens when the countdown ends
   - Customize labels and formatting

### Best Practices

To maximize the effectiveness of your announcements while maintaining a positive user experience, follow these best practices:

1. **Content Optimization**:
   - Keep messages clear, concise, and action-oriented
   - Use compelling call-to-action text that creates urgency or interest
   - Ensure text is readable against the background color
   - Limit announcements to one key message rather than cramming multiple offers
   - Use proper grammar and spelling to maintain professionalism

2. **Design Considerations**:
   - Match announcement design with your website's branding
   - Use contrasting colors to make announcements stand out without being jarring
   - Ensure text size is readable on all devices
   - Leave sufficient padding around text and buttons
   - Test appearance across different screen sizes [Screenshot Recommended]

3. **Strategic Timing and Targeting**:
   - Avoid showing too many announcements simultaneously
   - Use scheduling to display time-sensitive offers only when relevant
   - Target announcements to the most appropriate audience segments
   - Consider user journey and intent when determining where announcements appear
   - Implement frequency capping to prevent announcement fatigue

4. **Performance Optimization**:
   - Compress images used in announcements
   - Minimize the use of complex animations on mobile devices
   - Test site loading speed with announcements active
   - Consider using asynchronous loading for announcements

## Document Library

### Description

Document Library Lite is a powerful WordPress plugin developed by Barn2 Plugins that creates a comprehensive document management system for the Tonga National Portal. This plugin enables administrators to organize, display, and manage various types of documents in a professional and user-friendly manner. It transforms the portal into a centralized repository where government documents, forms, reports, and publications can be easily accessed, searched, and downloaded by citizens and staff.

The plugin creates a dedicated 'Documents' section in the WordPress admin panel, separate from other content types, allowing for specialized document management. Documents are displayed in a clean, responsive table layout that makes it easy for users to find what they need through instant search, sorting, and filtering capabilities.

Document Library Lite serves as an essential tool for government transparency and information dissemination, making it simple for the Tonga National Portal to maintain an organized collection of official documents that citizens can access anytime.

![Document Library main interface showing the table layout of documents](./../images/tnp/pf6.png)

### Key Features

- **Dedicated Document Management**: Creates a separate 'Documents' custom post type in WordPress admin for specialized document handling and organization.

- **Table Layout Display**: Presents documents in a clean, professional table format that's easy to navigate and scan.

- **Instant Search Functionality**: Allows users to quickly find specific documents by typing keywords that filter results in real-time.

- **Sorting and Filtering**: Enables users to sort documents by various criteria (date, title, etc.) and filter by categories or tags.

- **Document Categories and Tags**: Provides robust organization options through hierarchical categories and flexible tagging systems.

- **File Type Support**: Handles various file formats including PDF, Word documents, Excel spreadsheets, PowerPoint presentations, images, and more.

- **Download Management**: Tracks and manages document downloads with direct download links for easy access.

- **Shortcode Integration**: Includes the `[doc_library]` shortcode to display document libraries anywhere on the portal.

- **Responsive Design**: Ensures the document library displays properly across all devices, from desktop computers to mobile phones.

- **User-Friendly Interface**: Features an intuitive interface that requires no technical knowledge for citizens to navigate and use.

- **SEO-Friendly Structure**: Improves document discoverability through search engines with proper document indexing.

### Adding a New Document

Adding documents to the Document Library Lite plugin involves a straightforward process that ensures proper organization and accessibility:

1. **Access the Documents Section**:
   - Log in to the WordPress admin panel
   - Navigate to "Documents" in the left sidebar menu
   - Click on "Add New" to create a new document entry
   ![Documents menu with Add New option highlighted](./../images/tnp/pf7.png)

2. **Enter Document Information**:
   - Add a descriptive title for the document
   - Write a summary or description in the main content area
   - (Optional) Add a shorter excerpt that will appear in the document library listing
   - Set a featured image if relevant to the document (e.g., cover page thumbnail)

3. **Upload the Document File**:
   - Scroll to the "Document Details" meta box
   - Click "Upload File" or drag and drop your file into the designated area
   - Supported formats include PDF, DOC, DOCX, XLS, XLSX, PPT, PPTX, and various image formats
   - For larger files, ensure they don't exceed the server's maximum upload size
   ![Document upload interface showing file selection options](./../images/tnp/pf8.png)

4. **Organize with Categories and Tags**:
   - Assign the document to relevant categories (e.g., "Forms", "Reports", "Legislation")
   - Add tags to further classify the document (e.g., "2023", "Tax", "Healthcare")
   - Categories provide hierarchical organization while tags offer flexible cross-referencing
   - Consider creating a consistent category structure for better organization

5. **Set Document Attributes**:
   - Configure additional document properties:
     - Document date (publication or effective date)
     - Version number (if applicable)
     - Status (Draft, Published, etc.)
     - Access level (Public, Registered Users, etc.)

6. **Preview and Publish**:
   - Use the "Preview" button to check how the document will appear
   - Click "Publish" to make the document live on the portal
   - Alternatively, use "Schedule" to set a future publication date

### Best Practices

To maximize the effectiveness of the Document Library and ensure a positive user experience, follow these best practices:

1. **Document Organization Strategy**:
   - Develop a clear, consistent categorization system before adding many documents
   - Create a balanced category hierarchy that's neither too shallow nor too deep
   - Use descriptive category names that make sense to citizens (avoid government jargon)
   - Implement a standardized tagging convention for cross-referencing related documents
   - Consider creating category-specific document libraries for different sections of the portal

2. **Document Naming and Metadata**:
   - Use clear, descriptive titles that include key information (e.g., "2023 Tax Filing Form - Individual")
   - Include relevant dates in document titles or descriptions when applicable
   - Write concise but informative document descriptions
   - Add complete metadata (categories, tags, dates) to improve searchability
   - Use consistent naming conventions across similar document types

3. **File Optimization**:
   - Convert text-heavy documents to searchable PDFs rather than image scans
   - Compress large files to improve download speeds without sacrificing quality
   - Use PDF bookmarks and table of contents for lengthy documents
   - Ensure documents are accessible according to web standards (proper headings, alt text, etc.)
   - Consider breaking very large documents into logical sections for easier consumption

---

## Location Weather Pro

### Description

Location Weather Pro is a premium WordPress plugin developed by ShapedPlugin, LLC that enables website administrators to display accurate, real-time weather information and forecasts for any location worldwide on their WordPress websites. The plugin retrieves weather data from OpenWeatherMap (OWM), a comprehensive weather data and forecast API provider, and presents it in visually appealing, customizable formats.

As a powerful weather forecasting solution, Location Weather Pro transforms the Tonga National Portal into an informative resource for citizens seeking current weather conditions, hourly updates, extended forecasts, and weather alerts. The plugin offers multiple display options including widgets, shortcodes, and block editor integration, making it flexible for various implementation needs across the portal.

![Location Weather Pro main interface showing weather display options](./../images/tnp/pf9.png)

### Key Features

- **Real-time Weather Data**: Displays current temperature, weather conditions, humidity, wind speed and direction, pressure, visibility, and more for any location worldwide.

- **Multiple Location Methods**: Supports various ways to specify locations:
  - City name (e.g., "Nuku'alofa, TO")
  - City ID (OpenWeatherMap unique city identifier)
  - ZIP code with country code
  - Geographic coordinates (latitude and longitude)
  - Auto-detect visitor's location (with user permission)

- **Comprehensive Forecast Options**:
  - Hourly forecasts (up to 48 hours)
  - Daily forecasts (up to 16 days)
  - Detailed weather parameters for each forecast period

- **Multiple Display Layouts**:
  - Vertical layout
  - Horizontal layout
  - Weather card design
  - Minimal design
  - Customizable templates

- **Advanced Customization**:
  - 5 different weather icon sets
  - Custom background images or colors
  - Typography controls (font family, size, color)
  - Responsive design settings for all devices
  - Temperature unit selection (°C or °F)
  - 12-hour or 24-hour time format

- **Weather Alerts Integration**: Displays severe weather alerts and warnings for the selected location.

- **Weather Map**: Interactive weather map showing precipitation, temperature, wind, and cloud layers.

- **Multilingual Support**: Compatible with WPML and other translation plugins for displaying weather information in multiple languages.

- **Shortcode System**: Easy implementation anywhere on the site using shortcodes with various parameters.

- **Widget Ready**: Drag-and-drop widget functionality for sidebars and widget areas.

### Configuration

Setting up Location Weather Pro involves several key steps to ensure accurate weather data display:

1. **API Key Configuration**:
   - Sign up for an OpenWeatherMap account at [openweathermap.org](https://openweathermap.org/)
   - Generate a free or paid API key from your OpenWeatherMap dashboard
   - Navigate to **Location Weather → Settings** in your WordPress admin
   - Enter your API key in the designated field and save changes
   ![API Key configuration screen showing where to enter the OpenWeatherMap API key](./../images/tnp/pf10.png)

2. **Creating a New Weather Display**:
   - Go to **Location Weather → Add New**
   - Enter a descriptive title for internal reference (e.g., "Nuku'alofa Current Weather")
   - Configure the following settings:

3. **Location Settings**:
   - Select your preferred location method:
     - **City Name**: Enter city name with optional country code (e.g., "Nuku'alofa, TO")
     - **City ID**: Enter the OpenWeatherMap city ID
     - **ZIP Code**: Enter ZIP/postal code with country code
     - **Coordinates**: Enter latitude and longitude values
     - **Auto Detect**: Enable automatic location detection based on visitor's browser
   - For the Tonga National Portal, setting specific locations for major Tongan cities is recommended

4. **Display Options**:
   - **Layout Selection**: Choose from vertical, horizontal, or card layouts
   - **Template**: Select from available design templates
   - **Weather Data**: Configure which weather elements to display:
     - Current conditions (temperature, description, icon)
     - Additional details (humidity, pressure, wind, etc.)
     - Forecast options (hourly, daily, or both)
     - Sunrise/sunset times
   ![Weather display layout options showing different templates](./../images/tnp/pf11.png)

5. **Customization Settings**:
   - **Weather Icons**: Choose from 5 different icon sets
   - **Colors**: Set background, text, and accent colors
   - **Typography**: Configure font family, size, and weight
   - **Responsive Behavior**: Adjust display for different screen sizes
   - **Units**: Select temperature unit (°C or °F) and time format (12h or 24h)

6. **Advanced Options**:
   - **Cache Duration**: Set how long weather data should be cached (recommended: 1-3 hours)
   - **Fallback Data**: Configure what to display if API is unavailable
   - **Custom CSS**: Add custom styling if needed
   - **Weather Alerts**: Enable/disable severe weather alerts display

7. **Save and Generate Shortcode**:
   - Click "Publish" to save your weather display configuration
   - The plugin will generate a unique shortcode for this specific weather display

### Adding a Weather Widget to Homepage

Adding a weather widget to the Tonga National Portal homepage provides visitors with immediate access to current weather conditions:

1. **Create a Dedicated Weather Display**:
   - Follow the steps in the Configuration section to create a new weather display
   - Optimize the display for widget use by selecting a compact layout
   - Focus on current conditions and short-term forecast for homepage display
   - Save the configuration and note the generated shortcode

2. **Widget Implementation Methods**:
   
   **Method 1: Using the Built-in Widget**:
   - Navigate to **Appearance → Widgets** in your WordPress admin
   - Find the "Location Weather Pro" widget in the available widgets list
   - Drag and drop it to your desired widget area (e.g., sidebar, header, or footer)
   - Select the previously created weather display from the dropdown menu
   - Configure widget-specific settings if needed:
     - Title (optional)
     - CSS class (optional)
   - Save the widget settings

   **Method 2: Using a Shortcode in a Text Widget**:
   - Navigate to **Appearance → Widgets**
   - Add a "Text" or "Custom HTML" widget to your desired widget area
   - Paste the shortcode generated for your weather display
   - Save the widget settings

   **Method 3: Using Bricks Page Builder**:
   - If using a page builder for the homepage (Bricks)
   - Add a "Shortcode" element to your homepage design
   - Paste the weather display shortcode into the element
   - Adjust the element's styling and positioning as needed

3. **Optimizing the Homepage Weather Widget**:
   - Keep the widget compact and focused on essential information
   - Consider using auto-location detection for personalized weather
   - Use a design that complements the portal's color scheme
   - Ensure responsive behavior works well on all devices
   - For the Tonga National Portal, prioritize displaying weather for the capital city by default

### Creating a Weather Page with Shortcode

Creating a dedicated weather page allows for more comprehensive weather information display:

1. **Create Multiple Weather Displays**:
   - Create separate weather displays for major locations in Tonga
   - Configure each with appropriate settings:
     - Detailed current conditions
     - Extended forecasts (hourly and daily)
     - Weather maps if desired
     - Weather alerts for each location
   - Save each configuration and note the generated shortcodes

2. **Create a New Weather Page**:
   - Navigate to **Pages → Add New** in your WordPress admin
   - Enter an appropriate title (e.g., "Tonga Weather Forecast")
   - Add descriptive introduction text explaining the weather service

3. **Implement Weather Displays Using Shortcodes**:
   - Use the basic shortcode format: `[location-weather-pro id="X"]` where X is the display ID
   - For multiple locations, add each shortcode in a structured layout
   - Example implementation:
   
   ```
   <h2>Nuku'alofa Weather</h2>
   [location-weather-pro id="1"]
   
   <h2>Neiafu Weather</h2>
   [location-weather-pro id="2"]
   
   <h2>Pangai Weather</h2>
   [location-weather-pro id="3"]
   ```

4. **Advanced Shortcode Parameters**:
   - Customize individual instances with additional parameters:
   - `[location-weather-pro id="1" forecast="daily" days="5"]` - Show 5-day forecast only
   - `[location-weather-pro id="2" current="true" forecast="false"]` - Show current conditions only
   - `[location-weather-pro id="3" units="imperial"]` - Override temperature units

5. **Enhance the Weather Page**:
   - Add explanatory text between weather displays
   - Include a legend explaining weather icons and terminology
   - Consider adding weather safety information or seasonal weather patterns
   - Link to official weather resources for additional information

### Best Practices

To maximize the effectiveness of Location Weather Pro on the Tonga National Portal while maintaining optimal performance and user experience:

1. **API Usage Optimization**:
   - Use appropriate cache settings (2-3 hours recommended) to reduce API calls
   - Monitor API usage to stay within limits of your OpenWeatherMap plan
   - Consider upgrading to a paid API plan for higher call limits during severe weather seasons
   - Implement fallback display options in case of API unavailability

2. **Performance Considerations**:
   - Limit the number of weather displays on a single page to prevent slow loading
   - Optimize weather icons and background images for web use
   - Enable browser caching for weather assets
   - Use minimal layouts on high-traffic pages
   - Test weather displays on low-bandwidth connections

3. **Design and User Experience**:
   - Maintain consistent design language across all weather displays
   - Ensure text has sufficient contrast against backgrounds for readability
   - Use appropriate font sizes, especially for temperature and important data
   - Implement responsive designs that work well on all devices
   - Consider cultural preferences for temperature units (Tonga uses Celsius)

---

## MxChat

### Description

### Key Features

### Configuration

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