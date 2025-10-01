<?php

// functions.php বা plugin file
add_action('after_switch_theme', 'create_reports_table');

function create_reports_table(){
    global $wpdb;
    $table_name = $wpdb->prefix . 'reports';

    $charset_collate =  $wpdb->get_charset_collate();

    $sql= "CREATE TABLE IF NOT EXISTS $table_name(
    id INT NOT NULL AUTO_INCREMENT,
    quantity INT NOT NULL,
    PRIMARY KEY (id)
    ) $charset_collate";

     require_once(ABSPATH . 'wp-admin/includes/upgrade.php');
     dbDelta($sql);
}


// functions.php বা plugin file

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

// Callback function: page content
function my_reports_page() {
    echo '<div class="wrap">';
    echo '<h1>Reports</h1>';
    echo '<p>Welcome to the Reports admin page.</p>';
    echo '</div>';
}

function add_report_page() {
    global $wpdb;
    $table_name = $wpdb->prefix . 'reports';

    // Form handle
    if(isset($_POST['quantity'])){
        $qty = intval($_POST['quantity']);

        global $wpdb;
        $table_name = $wpdb->prefix . 'reports';

        $wpdb->insert(
            $table_name,
            array(
                'quantity'=> $qty,
            ),
            array(
                '%d',
            ),
        );

        echo '<div class="notice notice-success"><p>report add success</p></div>';

    }

    // Form
    echo '<div class="wrap">';
    echo '<h1>Add Report</h1>';
    echo '<form method="post">';
    echo '<input type="number" name="quantity" placeholder="Quantity" required>';
    echo '<button type="submit" name="save_report" class="button button-primary">Save</button>';
    echo '</form>';

    // Table data show

    $results = $wpdb->get_results("SELECT * FROM $table_name", ARRAY_A);
    if($results){
        echo "<h2>All reports</h2>";
        echo '<table class="wp-list-table widefat fixed striped">';
        echo '<thead><tr><th>ID</th><th>Quantity</th></tr></thead>';

        foreach($results as $row){
            echo '<tr>';
            echo '<td>'.esc_html($row['id']).'</td>';
            echo '<td>'.esc_html($row['quantity']).'</td>';
            echo '</tr>';
        }
        echo '</tbody></table>';
    }else{
        echo '<p>No reports found</p>';
    }

    echo '</div>';
}
