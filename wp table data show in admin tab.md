function add_report_page() {

global $wpdb;
$table_name = $wpdb->prefix . 'reports';

  // Table data show
    $results = $wpdb->get_results("SELECT * FROM $table_name", ARRAY_A);
    if($results) {
        echo '<h2>All Reports</h2>';
        echo '<table class="wp-list-table widefat fixed striped">';
        echo '<thead><tr><th>ID</th><th>Quantity</th></tr></thead><tbody>';
        foreach($results as $row) {
            echo '<tr>';
            echo '<td>'.esc_html($row['id']).'</td>';
            echo '<td>'.esc_html($row['quantity']).'</td>';
            echo '</tr>';
        }
        echo '</tbody></table>';
    } else {
        echo '<p>No reports found.</p>';
    }

    echo '</div>';
}
