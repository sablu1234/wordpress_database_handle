<?php
/*
Plugin Name: Reports
Description: Daily Reports Plugin
Version: 1.0
Author: Hasan
*/

// Prevent direct access
if ( ! defined('ABSPATH') ) exit;

// Include table function
include_once ('file/table.php');

// Register activation hook
register_activation_hook(__FILE__, 'reports_create_table');

include_once ('file/admin.php');

//table.php =============================>
<?php

function reports_create_table(){
    global $wpdb;
    $table_name = $wpdb->prefix . 'daily_reports';

    $charset_collate = $wpdb->get_charset_collate();

    $sql = "CREATE TABLE IF NOT EXISTS $table_name (
    id mediumint(9) NOT NULL AUTO_INCREMENT,
    quantity int NOT NULL DEFAULT 0,
    PRIMARY KEY (id)
    ) $charset_collate; ";

    require_once(ABSPATH . 'wp-admin/includes/upgrade.php');
    dbDelta($sql);
}


//admin.php========================>
<?php
/**
 * Register a custom menu page.
 */
add_action( 'admin_menu', 'reports_admin_menu' );
function reports_admin_menu() {
	add_menu_page(
		__( 'Reports', 'textdomain' ),//page title
		'Reports', //menu title
		'manage_options', //capability
		'reports',//slut
		'reports_admin_page',//callback
		'dashicons-analytics',//Icon
		25//position
	);
}



function reports_admin_page(){
    global $wpdb;
    $table_name = $wpdb->prefix . 'daily_reports';

    if(isset($_POST['add_quantity'])){
        $quantity = intval($_POST['quantity']);
        if($quantity > 0){
            $wpdb->insert(
                $table_name,
                ['quantity' => $quantity],
                ['%d']
            );
            echo 'insert data successful';
        }
    }
    ?>
    <h1>Add New Quantity</h1>
    <form  method="post">
        <p>
            <label for="">Quantity: </label>
            <input type="number" name="quantity" id= "quantity">
        </p>
        <p>
            <button type="submit" name="add_quantity" class="button button-primary">Add Quantity</button>
        </p>
    </form>
    
    <?php


    // === FETCH DATA ===
    $results =  $wpdb->get_results("SELECT * FROM $table_name ORDER BY ID DESC", ARRAY_A);
    ?>
        <div class="wrap">
            <h2>All Reports</h2>
            <table class="wp-list-table widefat fixed striped">
                <thead>
                    <tr>
                        <th>ID</th>
                        <th>Quantity</th>
                        <th>Action</th>
                    </tr>
                </thead>
                <tbody>
                    <?php
                    if($results){
                        foreach($results as $row){ ?>
                            <tr>
                                <td><?php echo esc_html($row['id']); ?></td>
                                <td><?php echo esc_html($row['quantity']); ?></td>
                                <td>
                                    <a href="?page=reports&edit_id=<?php echo $row['id']?>">Edit</a>
                                    <a href="?page=reports&delete=<?php echo $row['id']; ?>" 
                                    onclick="return confirm('Delete this item?')" >Delete</a>
                                </td>
                                </td>
                            </tr>
                        <?php }
                    } else {
                        echo '<tr><td colspan="3">No data found.</td></tr>';
                    }
                    ?>
                </tbody>
            </table>
        </div>
    <?php


// get update row
    
if(isset($_GET['edit_id'])){
    $edit_id = absint($_GET['edit_id']);
    $row = $wpdb->get_row($wpdb->prepare("SELECT * FROM $table_name WHERE id= %d",$edit_id), ARRAY_A);

    if($row){
        ?>
        <h2>Edit Quantity (ID: <?php echo $row['id']?>)</h2>
        <form action="" method="post">
            <input type="hidden" name="update_id" value="<?php echo $row['id']?>">
            <p>
                <label for="quantity">Quantity:</label>
                <input type="number" name="quantity" id="quantity" value="<?php echo $row['quantity']?>">
            </p>
            <p>
                <button type="submit" name="update_quantity" class="button button-primary">Update Quantity</button>
            </p>
        </form>
        <?php
    }
}

// update table
if( isset($_POST['update_quantity']) && isset($_POST['update_id'])){
    $update_id = absint($_POST['update_id']);
    $update_quantity = absint($_POST['quantity']);

    if($update_id > 0 && $update_quantity>=0){
        $wpdb->update(
            $table_name,
            ['quantity' => $update_quantity],
            ['id' => $update_id],
            ['%d'],
            ['%d'],
        );
    }
}

 // === DELETE ===
    if(isset($_GET['delete'])){
        $id = intval($_GET['delete']);
        $wpdb->delete($table_name, ['id'=> $id]);
    }
  

}

