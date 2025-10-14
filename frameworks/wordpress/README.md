# WordPress Development Guide

## 🎯 AI Agent Instructions for WordPress Development

When working with WordPress projects, follow these guidelines to ensure optimal AI assistance and code quality.

## 📁 Project Structure

```
wp-content/
├── themes/
│   └── your-theme/
│       ├── style.css
│       ├── index.php
│       ├── functions.php
│       ├── assets/
│       │   ├── css/
│       │   ├── js/
│       │   └── images/
│       ├── inc/
│       │   ├── enqueue.php
│       │   ├── customizer.php
│       │   └── template-functions.php
│       ├── template-parts/
│       └── page-templates/
├── plugins/
│   └── your-plugin/
│       ├── your-plugin.php
│       ├── includes/
│       ├── admin/
│       ├── public/
│       └── assets/
└── uploads/
```

## 🎨 Theme Development

### Theme Header Template
```php
<?php
/**
 * Theme Name: Your Theme Name
 * Description: A modern WordPress theme
 * Version: 1.0.0
 * Author: Your Name
 * Text Domain: your-theme
 * Domain Path: /languages
 */

// Prevent direct access
if (!defined('ABSPATH')) {
    exit;
}

// Define theme constants
define('THEME_VERSION', '1.0.0');
define('THEME_PATH', get_template_directory());
define('THEME_URL', get_template_directory_uri());
```

### Functions.php Template
```php
<?php
// functions.php

// Prevent direct access
if (!defined('ABSPATH')) {
    exit;
}

// Theme setup
function your_theme_setup() {
    // Add theme support
    add_theme_support('post-thumbnails');
    add_theme_support('title-tag');
    add_theme_support('html5', [
        'search-form',
        'comment-form',
        'comment-list',
        'gallery',
        'caption',
    ]);
    add_theme_support('custom-logo');
    add_theme_support('customize-selective-refresh-widgets');
    
    // Register navigation menus
    register_nav_menus([
        'primary' => __('Primary Menu', 'your-theme'),
        'footer' => __('Footer Menu', 'your-theme'),
    ]);
    
    // Add image sizes
    add_image_size('hero-image', 1200, 600, true);
    add_image_size('card-image', 400, 300, true);
}
add_action('after_setup_theme', 'your_theme_setup');

// Enqueue scripts and styles
function your_theme_scripts() {
    wp_enqueue_style(
        'your-theme-style',
        get_stylesheet_uri(),
        [],
        THEME_VERSION
    );
    
    wp_enqueue_script(
        'your-theme-script',
        THEME_URL . '/assets/js/main.js',
        ['jquery'],
        THEME_VERSION,
        true
    );
    
    // Localize script for AJAX
    wp_localize_script('your-theme-script', 'ajax_object', [
        'ajax_url' => admin_url('admin-ajax.php'),
        'nonce' => wp_create_nonce('your_theme_nonce'),
    ]);
}
add_action('wp_enqueue_scripts', 'your_theme_scripts');

// Register widget areas
function your_theme_widgets_init() {
    register_sidebar([
        'name' => __('Sidebar', 'your-theme'),
        'id' => 'sidebar-1',
        'description' => __('Add widgets here.', 'your-theme'),
        'before_widget' => '<section id="%1$s" class="widget %2$s">',
        'after_widget' => '</section>',
        'before_title' => '<h2 class="widget-title">',
        'after_title' => '</h2>',
    ]);
}
add_action('widgets_init', 'your_theme_widgets_init');

// Custom post types
function your_theme_custom_post_types() {
    register_post_type('portfolio', [
        'labels' => [
            'name' => __('Portfolio', 'your-theme'),
            'singular_name' => __('Portfolio Item', 'your-theme'),
        ],
        'public' => true,
        'has_archive' => true,
        'supports' => ['title', 'editor', 'thumbnail', 'excerpt'],
        'menu_icon' => 'dashicons-portfolio',
        'rewrite' => ['slug' => 'portfolio'],
    ]);
}
add_action('init', 'your_theme_custom_post_types');

// Custom fields (using ACF or custom meta boxes)
function your_theme_add_meta_boxes() {
    add_meta_box(
        'portfolio_details',
        __('Portfolio Details', 'your-theme'),
        'your_theme_portfolio_meta_box_callback',
        'portfolio',
        'normal',
        'high'
    );
}
add_action('add_meta_boxes', 'your_theme_add_meta_boxes');

function your_theme_portfolio_meta_box_callback($post) {
    wp_nonce_field('your_theme_portfolio_meta_box', 'your_theme_portfolio_meta_box_nonce');
    
    $client = get_post_meta($post->ID, '_portfolio_client', true);
    $url = get_post_meta($post->ID, '_portfolio_url', true);
    
    echo '<table class="form-table">';
    echo '<tr>';
    echo '<th><label for="portfolio_client">' . __('Client', 'your-theme') . '</label></th>';
    echo '<td><input type="text" id="portfolio_client" name="portfolio_client" value="' . esc_attr($client) . '" size="50" /></td>';
    echo '</tr>';
    echo '<tr>';
    echo '<th><label for="portfolio_url">' . __('Project URL', 'your-theme') . '</label></th>';
    echo '<td><input type="url" id="portfolio_url" name="portfolio_url" value="' . esc_attr($url) . '" size="50" /></td>';
    echo '</tr>';
    echo '</table>';
}

// Save custom fields
function your_theme_save_portfolio_meta($post_id) {
    if (!isset($_POST['your_theme_portfolio_meta_box_nonce'])) {
        return;
    }
    
    if (!wp_verify_nonce($_POST['your_theme_portfolio_meta_box_nonce'], 'your_theme_portfolio_meta_box')) {
        return;
    }
    
    if (defined('DOING_AUTOSAVE') && DOING_AUTOSAVE) {
        return;
    }
    
    if (isset($_POST['post_type']) && 'portfolio' == $_POST['post_type']) {
        if (!current_user_can('edit_page', $post_id)) {
            return;
        }
    } else {
        if (!current_user_can('edit_post', $post_id)) {
            return;
        }
    }
    
    if (isset($_POST['portfolio_client'])) {
        update_post_meta($post_id, '_portfolio_client', sanitize_text_field($_POST['portfolio_client']));
    }
    
    if (isset($_POST['portfolio_url'])) {
        update_post_meta($post_id, '_portfolio_url', esc_url_raw($_POST['portfolio_url']));
    }
}
add_action('save_post', 'your_theme_save_portfolio_meta');
```

### Template Hierarchy

#### index.php (Main Template)
```php
<?php get_header(); ?>

<main id="main" class="site-main">
    <?php if (have_posts()) : ?>
        <div class="posts-container">
            <?php while (have_posts()) : the_post(); ?>
                <article id="post-<?php the_ID(); ?>" <?php post_class('post-card'); ?>>
                    <?php if (has_post_thumbnail()) : ?>
                        <div class="post-thumbnail">
                            <a href="<?php the_permalink(); ?>">
                                <?php the_post_thumbnail('card-image'); ?>
                            </a>
                        </div>
                    <?php endif; ?>
                    
                    <div class="post-content">
                        <header class="post-header">
                            <h2 class="post-title">
                                <a href="<?php the_permalink(); ?>"><?php the_title(); ?></a>
                            </h2>
                            <div class="post-meta">
                                <span class="post-date"><?php echo get_the_date(); ?></span>
                                <span class="post-author"><?php the_author(); ?></span>
                            </div>
                        </header>
                        
                        <div class="post-excerpt">
                            <?php the_excerpt(); ?>
                        </div>
                        
                        <footer class="post-footer">
                            <a href="<?php the_permalink(); ?>" class="read-more">
                                <?php _e('Read More', 'your-theme'); ?>
                            </a>
                        </footer>
                    </div>
                </article>
            <?php endwhile; ?>
        </div>
        
        <?php
        // Pagination
        the_posts_pagination([
            'prev_text' => __('Previous', 'your-theme'),
            'next_text' => __('Next', 'your-theme'),
        ]);
        ?>
    <?php else : ?>
        <div class="no-posts">
            <h2><?php _e('Nothing Found', 'your-theme'); ?></h2>
            <p><?php _e('It seems we can\'t find what you\'re looking for.', 'your-theme'); ?></p>
        </div>
    <?php endif; ?>
</main>

<?php get_sidebar(); ?>
<?php get_footer(); ?>
```

#### single.php (Single Post Template)
```php
<?php get_header(); ?>

<main id="main" class="site-main">
    <?php while (have_posts()) : the_post(); ?>
        <article id="post-<?php the_ID(); ?>" <?php post_class('single-post'); ?>>
            <header class="post-header">
                <h1 class="post-title"><?php the_title(); ?></h1>
                <div class="post-meta">
                    <span class="post-date"><?php echo get_the_date(); ?></span>
                    <span class="post-author"><?php the_author(); ?></span>
                    <span class="post-categories"><?php the_category(', '); ?></span>
                </div>
            </header>
            
            <?php if (has_post_thumbnail()) : ?>
                <div class="post-featured-image">
                    <?php the_post_thumbnail('hero-image'); ?>
                </div>
            <?php endif; ?>
            
            <div class="post-content">
                <?php the_content(); ?>
                
                <?php
                wp_link_pages([
                    'before' => '<div class="page-links">' . __('Pages:', 'your-theme'),
                    'after' => '</div>',
                ]);
                ?>
            </div>
            
            <footer class="post-footer">
                <div class="post-tags">
                    <?php the_tags('<span class="tags-label">' . __('Tags:', 'your-theme') . '</span> ', ', ', ''); ?>
                </div>
                
                <div class="post-navigation">
                    <?php
                    the_post_navigation([
                        'prev_text' => '<span class="nav-subtitle">' . __('Previous:', 'your-theme') . '</span> <span class="nav-title">%title</span>',
                        'next_text' => '<span class="nav-subtitle">' . __('Next:', 'your-theme') . '</span> <span class="nav-title">%title</span>',
                    ]);
                    ?>
                </div>
            </footer>
        </article>
        
        <?php
        // Comments
        if (comments_open() || get_comments_number()) :
            comments_template();
        endif;
        ?>
    <?php endwhile; ?>
</main>

<?php get_sidebar(); ?>
<?php get_footer(); ?>
```

## 🔌 Plugin Development

### Plugin Header Template
```php
<?php
/**
 * Plugin Name: Your Plugin Name
 * Plugin URI: https://yourwebsite.com/your-plugin
 * Description: A brief description of your plugin
 * Version: 1.0.0
 * Author: Your Name
 * Author URI: https://yourwebsite.com
 * License: GPL v2 or later
 * License URI: https://www.gnu.org/licenses/gpl-2.0.html
 * Text Domain: your-plugin
 * Domain Path: /languages
 */

// Prevent direct access
if (!defined('ABSPATH')) {
    exit;
}

// Define plugin constants
define('YOUR_PLUGIN_VERSION', '1.0.0');
define('YOUR_PLUGIN_PATH', plugin_dir_path(__FILE__));
define('YOUR_PLUGIN_URL', plugin_dir_url(__FILE__));
define('YOUR_PLUGIN_BASENAME', plugin_basename(__FILE__));
```

### Plugin Class Structure
```php
<?php
// includes/class-your-plugin.php

class Your_Plugin {
    
    private static $instance = null;
    
    public static function get_instance() {
        if (null === self::$instance) {
            self::$instance = new self();
        }
        return self::$instance;
    }
    
    private function __construct() {
        add_action('init', [$this, 'init']);
        add_action('wp_enqueue_scripts', [$this, 'enqueue_scripts']);
        add_action('admin_enqueue_scripts', [$this, 'admin_enqueue_scripts']);
        add_action('wp_ajax_your_plugin_action', [$this, 'handle_ajax_action']);
        add_action('wp_ajax_nopriv_your_plugin_action', [$this, 'handle_ajax_action']);
        
        register_activation_hook(__FILE__, [$this, 'activate']);
        register_deactivation_hook(__FILE__, [$this, 'deactivate']);
    }
    
    public function init() {
        // Load text domain
        load_plugin_textdomain('your-plugin', false, dirname(YOUR_PLUGIN_BASENAME) . '/languages');
        
        // Register custom post types
        $this->register_post_types();
        
        // Register custom fields
        $this->register_custom_fields();
    }
    
    public function enqueue_scripts() {
        wp_enqueue_style(
            'your-plugin-style',
            YOUR_PLUGIN_URL . 'assets/css/public.css',
            [],
            YOUR_PLUGIN_VERSION
        );
        
        wp_enqueue_script(
            'your-plugin-script',
            YOUR_PLUGIN_URL . 'assets/js/public.js',
            ['jquery'],
            YOUR_PLUGIN_VERSION,
            true
        );
        
        wp_localize_script('your-plugin-script', 'your_plugin_ajax', [
            'ajax_url' => admin_url('admin-ajax.php'),
            'nonce' => wp_create_nonce('your_plugin_nonce'),
        ]);
    }
    
    public function admin_enqueue_scripts($hook) {
        // Only load on specific admin pages
        if ('post.php' !== $hook && 'post-new.php' !== $hook) {
            return;
        }
        
        wp_enqueue_style(
            'your-plugin-admin-style',
            YOUR_PLUGIN_URL . 'assets/css/admin.css',
            [],
            YOUR_PLUGIN_VERSION
        );
        
        wp_enqueue_script(
            'your-plugin-admin-script',
            YOUR_PLUGIN_URL . 'assets/js/admin.js',
            ['jquery'],
            YOUR_PLUGIN_VERSION,
            true
        );
    }
    
    public function handle_ajax_action() {
        // Verify nonce
        if (!wp_verify_nonce($_POST['nonce'], 'your_plugin_nonce')) {
            wp_die('Security check failed');
        }
        
        // Check user permissions
        if (!current_user_can('manage_options')) {
            wp_die('Insufficient permissions');
        }
        
        // Process the AJAX request
        $result = $this->process_ajax_request($_POST);
        
        wp_send_json_success($result);
    }
    
    private function process_ajax_request($data) {
        // Your AJAX processing logic here
        return ['message' => 'Success'];
    }
    
    public function activate() {
        // Create database tables
        $this->create_tables();
        
        // Set default options
        $this->set_default_options();
        
        // Flush rewrite rules
        flush_rewrite_rules();
    }
    
    public function deactivate() {
        // Clean up on deactivation
        flush_rewrite_rules();
    }
    
    private function create_tables() {
        global $wpdb;
        
        $table_name = $wpdb->prefix . 'your_plugin_table';
        
        $charset_collate = $wpdb->get_charset_collate();
        
        $sql = "CREATE TABLE $table_name (
            id mediumint(9) NOT NULL AUTO_INCREMENT,
            name varchar(100) NOT NULL,
            email varchar(100) NOT NULL,
            created_at datetime DEFAULT CURRENT_TIMESTAMP,
            PRIMARY KEY (id)
        ) $charset_collate;";
        
        require_once(ABSPATH . 'wp-admin/includes/upgrade.php');
        dbDelta($sql);
    }
    
    private function set_default_options() {
        $default_options = [
            'your_plugin_option_1' => 'default_value_1',
            'your_plugin_option_2' => 'default_value_2',
        ];
        
        foreach ($default_options as $option => $value) {
            if (get_option($option) === false) {
                add_option($option, $value);
            }
        }
    }
}

// Initialize the plugin
Your_Plugin::get_instance();
```

## 🎨 Customizer Integration

### Customizer Template
```php
<?php
// inc/customizer.php

function your_theme_customize_register($wp_customize) {
    // Add custom section
    $wp_customize->add_section('your_theme_options', [
        'title' => __('Theme Options', 'your-theme'),
        'priority' => 30,
    ]);
    
    // Add custom setting
    $wp_customize->add_setting('hero_title', [
        'default' => __('Welcome to Our Site', 'your-theme'),
        'sanitize_callback' => 'sanitize_text_field',
        'transport' => 'postMessage',
    ]);
    
    // Add custom control
    $wp_customize->add_control('hero_title', [
        'label' => __('Hero Title', 'your-theme'),
        'section' => 'your_theme_options',
        'type' => 'text',
    ]);
    
    // Add color setting
    $wp_customize->add_setting('primary_color', [
        'default' => '#007cba',
        'sanitize_callback' => 'sanitize_hex_color',
        'transport' => 'postMessage',
    ]);
    
    $wp_customize->add_control(new WP_Customize_Color_Control($wp_customize, 'primary_color', [
        'label' => __('Primary Color', 'your-theme'),
        'section' => 'your_theme_options',
    ]));
    
    // Add image setting
    $wp_customize->add_setting('hero_image', [
        'default' => '',
        'sanitize_callback' => 'absint',
    ]);
    
    $wp_customize->add_control(new WP_Customize_Media_Control($wp_customize, 'hero_image', [
        'label' => __('Hero Image', 'your-theme'),
        'section' => 'your_theme_options',
        'mime_type' => 'image',
    ]));
}
add_action('customize_register', 'your_theme_customize_register');

// Add selective refresh support
function your_theme_customize_preview_js() {
    wp_enqueue_script(
        'your-theme-customizer',
        THEME_URL . '/assets/js/customizer.js',
        ['customize-preview'],
        THEME_VERSION,
        true
    );
}
add_action('customize_preview_init', 'your_theme_customize_preview_js');
```

## 🧪 Testing Patterns

### Unit Testing with PHPUnit
```php
<?php
// tests/TestYourPlugin.php

class TestYourPlugin extends WP_UnitTestCase {
    
    public function setUp() {
        parent::setUp();
        $this->plugin = new Your_Plugin();
    }
    
    public function test_plugin_initialization() {
        $this->assertInstanceOf('Your_Plugin', $this->plugin);
    }
    
    public function test_custom_post_type_registration() {
        $post_types = get_post_types();
        $this->assertArrayHasKey('portfolio', $post_types);
    }
    
    public function test_ajax_handler() {
        // Set up AJAX request
        $_POST['nonce'] = wp_create_nonce('your_plugin_nonce');
        $_POST['action'] = 'your_plugin_action';
        
        // Mock user capabilities
        $user = $this->factory->user->create(['role' => 'administrator']);
        wp_set_current_user($user);
        
        // Test AJAX handler
        try {
            $this->_handleAjax('your_plugin_action');
        } catch (WPAjaxDieContinueException $e) {
            // Expected for successful AJAX
        }
        
        $response = json_decode($this->_last_response, true);
        $this->assertTrue($response['success']);
    }
}
```

## 🔧 Common Patterns

### Shortcode Template
```php
<?php
// Register shortcode
add_shortcode('your_shortcode', 'your_shortcode_callback');

function your_shortcode_callback($atts) {
    $atts = shortcode_atts([
        'title' => 'Default Title',
        'count' => 5,
        'category' => '',
    ], $atts, 'your_shortcode');
    
    ob_start();
    ?>
    <div class="your-shortcode">
        <h3><?php echo esc_html($atts['title']); ?></h3>
        <?php
        $args = [
            'post_type' => 'post',
            'posts_per_page' => intval($atts['count']),
        ];
        
        if (!empty($atts['category'])) {
            $args['category_name'] = sanitize_text_field($atts['category']);
        }
        
        $query = new WP_Query($args);
        
        if ($query->have_posts()) :
            while ($query->have_posts()) : $query->the_post();
                ?>
                <div class="shortcode-post">
                    <h4><a href="<?php the_permalink(); ?>"><?php the_title(); ?></a></h4>
                    <p><?php the_excerpt(); ?></p>
                </div>
                <?php
            endwhile;
            wp_reset_postdata();
        endif;
        ?>
    </div>
    <?php
    return ob_get_clean();
}
```

### Widget Template
```php
<?php
// Register widget
add_action('widgets_init', 'register_your_widget');

function register_your_widget() {
    register_widget('Your_Widget');
}

class Your_Widget extends WP_Widget {
    
    public function __construct() {
        parent::__construct(
            'your_widget',
            __('Your Widget', 'your-theme'),
            ['description' => __('A custom widget description', 'your-theme')]
        );
    }
    
    public function widget($args, $instance) {
        $title = apply_filters('widget_title', $instance['title']);
        
        echo $args['before_widget'];
        
        if (!empty($title)) {
            echo $args['before_title'] . $title . $args['after_title'];
        }
        
        // Widget content
        echo '<div class="your-widget-content">';
        echo '<p>Widget content goes here</p>';
        echo '</div>';
        
        echo $args['after_widget'];
    }
    
    public function form($instance) {
        $title = !empty($instance['title']) ? $instance['title'] : __('New title', 'your-theme');
        ?>
        <p>
            <label for="<?php echo $this->get_field_id('title'); ?>"><?php _e('Title:', 'your-theme'); ?></label>
            <input class="widefat" id="<?php echo $this->get_field_id('title'); ?>" name="<?php echo $this->get_field_name('title'); ?>" type="text" value="<?php echo esc_attr($title); ?>">
        </p>
        <?php
    }
    
    public function update($new_instance, $old_instance) {
        $instance = [];
        $instance['title'] = (!empty($new_instance['title'])) ? strip_tags($new_instance['title']) : '';
        
        return $instance;
    }
}
```

## 🚨 Common Pitfalls

1. **Security vulnerabilities**: Always sanitize and validate user input
2. **Performance issues**: Use proper caching and optimize database queries
3. **Plugin conflicts**: Use unique prefixes and proper namespacing
4. **Theme updates**: Use child themes to preserve customizations
5. **Database queries**: Use WordPress functions instead of direct SQL when possible

## 📚 Recommended Tools

- **Development**: Local by Flywheel, XAMPP, Docker
- **Code Quality**: PHPCS, PHPStan, WordPress Coding Standards
- **Version Control**: Git, GitHub, GitLab
- **Deployment**: WP-CLI, Composer, Capistrano
- **Testing**: PHPUnit, Codeception, WP Browser
- **Performance**: Query Monitor, P3 Profiler, New Relic

## 🎯 AI Agent Best Practices

When assisting with WordPress development:

1. **Follow WordPress Coding Standards** for consistency
2. **Use proper security practices** (sanitization, validation, nonces)
3. **Implement proper error handling** and logging
4. **Use WordPress hooks and filters** appropriately
5. **Optimize for performance** with caching and efficient queries
6. **Write comprehensive tests** for custom functionality
7. **Follow accessibility guidelines** (WCAG compliance)
8. **Use proper internationalization** with text domains
9. **Implement proper database interactions** with WordPress APIs
10. **Follow plugin and theme development best practices**

## 🔗 Additional Resources

- [WordPress Codex](https://codex.wordpress.org/)
- [WordPress Developer Handbook](https://developer.wordpress.org/)
- [WordPress Coding Standards](https://developer.wordpress.org/coding-standards/)
- [WordPress Plugin API](https://developer.wordpress.org/plugins/)
- [WordPress Theme Development](https://developer.wordpress.org/themes/)
