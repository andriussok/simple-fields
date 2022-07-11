# simple-fields

Manage your info in a single place, access it with shortcode attributes.

```
/**
 * Simple fields.
 *
 * Create single source data accessible via shortcode attributes,
 * which can be used like global variables to dispay data in frontend.
 *
 * @param array  $atts      The user defined shortcode attributes.
 * @var   array  $pairs     The supported attributes and their defaults.
 *
 * To get $pair key value use [ shortcode_tag shortcode_attr ]
 * shortcode example [ prefix_contact phone ] will return phone number.
 */
add_shortcode( 'prefix_contact', 'prefix_contact_cb' );
function prefix_contact_cb( $atts, $content, $tag ) {
  $pairs = shortcode_atts(
           array(
           'address'    => '20 Dummy Road, Bham, UK',
           'open_hours' => '8:00am - 5:00pm',
           'phone'      => '+44 0123 456 789',
           'email'      => 'example@example.com',
           'facebook'   => 'https://fb.com/',
           'linkedin'   => 'https://linkedin.com/',
           ), $atts, 'prefix_contact' );
  
  // Define key
  $att = $atts ? $atts[0] : null; // first attribute passed in shortcode.
  $not_in_array = !in_array( $att, array_keys($pairs) ); // is attribute a valid array key.

  // Test passed attributre.
  if( empty( $atts ) || $not_in_array ) {
   
   // OPTIONAL message only for admins.
   if( current_user_can( 'manage_options' ) ) {
      $msg = 'Your shortcode <strong>'.$tag.'</strong> must have a valid attribute. ';
      $msg .= 'Current attribute ';
      if( empty( $atts ) ) $msg .= 'is <strong>empty.</strong> ';
      if(!empty( $atts ) && $not_in_array ) $msg .= '<strong>'.$att.'</strong> is invalid. ';
      $msg .= '<br>Please check valid attributes in pairs array ';
      trigger_error( $msg );
   }

   return; // End test, terminate.
  }

  // Get $pairs single key value.
  $str = $pairs[ $att ];
  
  // Maybe_sanitise.
  $arr_html = array( 'br' => array(), 'span' => array() );
  $str = wp_kses( $str, $arr_html );

  ob_start(); // Start buffer.
  echo $str;
  return ob_get_clean(); // Clean buffer, return value.
}```
