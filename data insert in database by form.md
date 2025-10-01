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
