---
description: Apply PHP code changes for Liine Integration (template-appointment.php and template-ppc.php)
---

This workflow applies the necessary PHP code modifications to support the Liine iFrame integration in the Allervie theme. 

## 1. Modifying `template-appointment.php`

**File:** `c:\wamp64\www\allervie\wp-content\themes\allervie\templates\template-appointment.php`

**Instructions:**
1. Fetch the ACF field for the new Liine iframe (`$alrv_tmp_app_liine_iframe`) globally along with the title.
2. Replace the Gravity form output section and URL parameter parsing logic (`isset($_GET['form'])`) with a conditional that outputs the `$alrv_tmp_app_liine_iframe` if it exists, otherwise falls back to the content page layout.
3. Remove the entire `<script>` block that contains the `populate_location`, `gform_input_change`, and related JavaScript. (Lines 49-302 approx, leaving just `<?php get_footer( 'app' );` at the end).

**Reference Diff:**
```diff
--- c:\wamp64\www\allervie\wp-content\themes\allervie\templates\template-appointment.php
+++ c:\wamp64\www\allervie\wp-content\themes\allervie\templates\template-appointment.php
@@ -19,6 +19,8 @@
 global $pID;
 global $fields;
 $alrv_tmp_app_title = ( isset( $fields['alrv_tmp_app_title'] ) ) ? $fields['alrv_tmp_app_title'] : null;
+$alrv_tmp_app_liine_iframe = ( isset( $fields['alrv_tmp_app_liine_iframe'] ) ) ? $fields['alrv_tmp_app_liine_iframe'] : null;
+
 if ( ! $alrv_tmp_app_title ) {
 	$alrv_tmp_app_title = get_the_title();
 }
@@ -28,7 +30,10 @@
 	<div class="appointment-form-ctn">
 		<?php
-		if(isset($_GET['form']) && $_GET['form']=='custom' ){
-			if(isset($_GET['app-location'])){
-				$post_fields=get_fields($_GET['app-location']);
-				$alrv_slo_external_form = ( isset( $post_fields['alrv_slo_external_form'] ) ) ? $post_fields['alrv_slo_external_form'] : null;
-				echo $alrv_slo_external_form;
-			}
+		if ($alrv_tmp_app_liine_iframe) {
+			// Output the new Liine iframe code directly if it exists
+			echo $alrv_tmp_app_liine_iframe;
 		}else{
 			while ( have_posts() ) {
 				the_post();
@@ -46,258 +51,2 @@
 	</div>
 </section>
-<script>
-var location_va = '';
- ... [Remove entire script block] ...
-</script>
 <?php
 get_footer( 'app' );
```

## 2. Modifying `template-ppc.php`

**File:** `c:\wamp64\www\allervie\wp-content\themes\allervie\templates\template-ppc.php`

**Instructions:**
1. In the variable declaration section at the top of the file, fetch the following new ACF fields:
   - `$alrv_tmp_ppc_liine_iframe`
   - `$alrv_tmp_ppc_tab_1_liine_iframe`
   - `$alrv_tmp_ppc_tab_2_liine_iframe`
2. In the hero form section (`$alrv_tmp_ppc_content_type == 'form'`), conditionally check for `$alrv_tmp_ppc_liine_iframe`. If it exists, output it; otherwise, fall back to the gravity form shortcode logic.
3. In Tab 1 (`$alrv_tmp_ppc_tab_1_content_type == 'form'`), conditionally check for `$alrv_tmp_ppc_tab_1_liine_iframe`. If it exists, output it; otherwise, fall back to the gravity form shortcode.
4. In Tab 2 (`$alrv_tmp_ppc_tab_2_content_type == 'form'`), conditionally check for `$alrv_tmp_ppc_tab_2_liine_iframe`. If it exists, output it; otherwise, fall back to the gravity form shortcode.

**Reference Diff:**
```diff
--- c:\wamp64\www\allervie\wp-content\themes\allervie\templates\template-ppc.php
+++ c:\wamp64\www\allervie\wp-content\themes\allervie\templates\template-ppc.php
@@ -32,6 +32,7 @@
 $alrv_tmp_ppc_form = (isset($fields['alrv_tmp_ppc_form'])) ? $fields['alrv_tmp_ppc_form'] : null;
 $alrv_tmp_ppc_form_title = (isset($fields['alrv_tmp_ppc_form_title'])) ? $fields['alrv_tmp_ppc_form_title'] : null;
 $alrv_tmp_ppc_from_text = (isset($fields['alrv_tmp_ppc_from_text'])) ? $fields['alrv_tmp_ppc_from_text'] : null;
+$alrv_tmp_ppc_liine_iframe = (isset($fields['alrv_tmp_ppc_liine_iframe'])) ? $fields['alrv_tmp_ppc_liine_iframe'] : null;
 
@@ -53,6 +54,7 @@
 $alrv_tmp_ppc_tab_1_form = (isset($fields['alrv_tmp_ppc_tab_1_form'])) ? $fields['alrv_tmp_ppc_tab_1_form'] : null;
+$alrv_tmp_ppc_tab_1_liine_iframe = (isset($fields['alrv_tmp_ppc_tab_1_liine_iframe'])) ? $fields['alrv_tmp_ppc_tab_1_liine_iframe'] : null;
 
@@ -64,6 +66,7 @@
 $alrv_tmp_ppc_tab_2_form = (isset($fields['alrv_tmp_ppc_tab_2_form'])) ? $fields['alrv_tmp_ppc_tab_2_form'] : null;
+$alrv_tmp_ppc_tab_2_liine_iframe = (isset($fields['alrv_tmp_ppc_tab_2_liine_iframe'])) ? $fields['alrv_tmp_ppc_tab_2_liine_iframe'] : null;
 
@@ -155,7 +158,9 @@
 							</div>
 								<?php
-								if($alrv_tmp_ppc_form){
+								if($alrv_tmp_ppc_liine_iframe){
+									echo $alrv_tmp_ppc_liine_iframe;
+								}elseif($alrv_tmp_ppc_form){
 									echo do_shortcode( '[gravityform id="' . $alrv_tmp_ppc_form . '" title=false description=false ajax=true]' );
 								}
 								?>
@@ -204,7 +209,11 @@
 															<?php if($alrv_tmp_ppc_tab_1_content_type == 'form') { ?>
 																<div class="content-row">
-								                					<?php echo do_shortcode( '[gravityform id="' . $alrv_tmp_ppc_tab_1_form . '" title=false description=false ajax=true]' ); ?>
+																	<?php if($alrv_tmp_ppc_tab_1_liine_iframe){
+																		echo $alrv_tmp_ppc_tab_1_liine_iframe;
+																	} else {
+																		echo do_shortcode( '[gravityform id="' . $alrv_tmp_ppc_tab_1_form . '" title=false description=false ajax=true]' );
+																	} ?>
 								                				</div>
@@ -232,7 +241,11 @@
 															<?php if($alrv_tmp_ppc_tab_2_content_type == 'form') { ?>
 																<div class="content-row">
-								                					<?php echo do_shortcode( '[gravityform id="' . $alrv_tmp_ppc_tab_2_form . '" title=false description=false ajax=true]' ); ?>
+																	<?php if($alrv_tmp_ppc_tab_2_liine_iframe){
+																		echo $alrv_tmp_ppc_tab_2_liine_iframe;
+																	} else {
+																		echo do_shortcode( '[gravityform id="' . $alrv_tmp_ppc_tab_2_form . '" title=false description=false ajax=true]' );
+																	} ?>
 								                				</div>
```

## IMPORTANT NOTES FOR EXECUTION:
1. Ensure to check if the exact lines still match, if something has changed adjust accordingly.
2. The removal of the `<script>` tag in `template-appointment.php` must be a singular bulk operation spanning from `<script>` through to `</script>`.
3. Do not create backend fields as part of this workflow; backend mapping via ACF is handled manually by WP Admin after these files are deployed.
