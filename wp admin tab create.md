add_action('admin_menu', 'my_reports_admin_menu');
function my_reports_admin_menu() {
    add_menu_page(
        'Reports',          // Page title
        'Reports',          // Menu title
        'manage_options',   // Capability
        'my-reports',       // Menu slug
        'my_reports_page',  // Callback function
        'dashicons-chart-bar', // Icon URL / Dashicon
        6                   // Position
    );

    add_submenu_page(
        'my-reports',       // Parent slug (main menu)
        'Add Report',       // Page title
        'Add Report',       // Menu title
        'manage_options',   // Capability
        'add-report',       // Submenu slug
        'add_report_page'   // Callback function
    );
}

function my_reports_page() {
    echo '<div class="wrap">';
    echo '<h1>Reports</h1>';
    echo '<p>Welcome to the Reports admin page.</p>';
    echo '</div>';
}

function add_report_page() {
    echo '<div class="wrap">';
    echo '<h1>Reports</h1>';
    echo '<p>Welcome to the Reports admin page.</p>';
    echo '</div>';
}
