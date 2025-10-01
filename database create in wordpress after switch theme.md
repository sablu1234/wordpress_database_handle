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
