# simple-fields

Manage your info state data in a single place and access it with shortcode attributes.

```
add_shortcode('was_contact', 'myprefix_contact_fn');
function myprefix_contact_fn($atts) {
  // Please use shortcode args array for company details
  $args = shortcode_atts(
        array(
        'address' => 'Birmingham, B0 B11',
        'phone' => '0123 456 789',
        'email' => 'example@example.com',
        'facebook' => 'https://fb.com/',
        'linkedin' => 'https://linkedin.com',
        ), $args, 'prefix_contact' );
  
  // check if attributes passed in shortcode
  if(empty($atts)) return;
  
  ob_start(); // start buffer
  echo $args[$atts[0]]; // get only single-first value
  return ob_get_clean(); // clean buffer, return value
     
//  use [myprefix_contact phone] to get global phone number
}
```
