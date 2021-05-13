# awesome-woocomerce



insert into functions.php of theme










### Hide Price Range for WooCommerce Variable Products
```php
//Hide Price Range for WooCommerce Variable Products
add_filter( 'woocommerce_variable_sale_price_html', 
'lw_variable_product_price', 10, 2 );
add_filter( 'woocommerce_variable_price_html', 
'lw_variable_product_price', 10, 2 );

function lw_variable_product_price( $v_price, $v_product ) {
```
### Re-add price
```php
// Product Price
$prod_prices = array( $v_product->get_variation_price( 'min', true ), 
                            $v_product->get_variation_price( 'max', true ) );
$prod_price = $prod_prices[0]!==$prod_prices[1] ? sprintf(__('%1$s', 'woocommerce'), 
                       wc_price( $prod_prices[0] ) ) : wc_price( $prod_prices[0] );

// Regular Price
$regular_prices = array( $v_product->get_variation_regular_price( 'min', true ), 
                          $v_product->get_variation_regular_price( 'max', true ) );
sort( $regular_prices );
$regular_price = $regular_prices[0]!==$regular_prices[1] ? sprintf(__('%1$s','woocommerce')
                      , wc_price( $regular_prices[0] ) ) : wc_price( $regular_prices[0] );

if ( $prod_price !== $regular_price ) {
$prod_price = '<del>'.$regular_price.$v_product->get_price_suffix() . '</del> <ins>' . 
                       $prod_price . $v_product->get_price_suffix() . '</ins>';
}
return $prod_price;
}
```
### Do not re-add products to cart when refresh the page
```php
// Do not re-add products to cart when refresh the page
add_action( 'woocommerce_add_to_cart_redirect', 'theme_slug_child_variation_link' );
function theme_slug_child_variation_link() {
    if( isset( $_SERVER[ 'HTTP_REFERER' ] ) ) {
        return $_SERVER[ 'HTTP_REFERER' ];
    }

}
```

### Aggiunge "mancano ancora" banner per spedizione gratuita
```php
add_action( 'woocommerce_before_cart', 'bbloomer_free_shipping_cart_notice' );
  
function bbloomer_free_shipping_cart_notice() {
  
   $min_amount = 97; //change this to your free shipping threshold
   
   $current = WC()->cart->subtotal;
  
   if ( $current < $min_amount ) {
      $added_text = 'Ottieni la spedizione, gratuita aggiungi altri ' . wc_price( $min_amount - $current ) . ' al tuo carrello!';
      $return_to = wc_get_page_permalink( 'shop' );
      $notice = sprintf( '<a href="%s" class="button wc-forward">%s</a> %s', esc_url( $return_to ), 'Continua lo Shopping', $added_text );
      wc_print_notice( $notice, 'notice' );
   }
  
}
```

### rimuove alcune voci dal form di checkout
```php
//
//
add_filter( 'woocommerce_checkout_fields' , 'custom_mod_checkout_fields' );
function custom_mod_checkout_fields( $fields ) {
unset($fields['billing']['billing_company']); // Azienda
unset($fields['order']['order_comments']); // Commenti ordine
return $fields;
}
```

# jquery 

### Replace placeholder with jquery 
(probably your theme will forces you tu use "$j" instead
```js
$('#this_is_your_id').attr('placeholder','Your New Text');
```

### move description before image on single product page
(probably your theme will forces you tu use "$j" instead
```js
jQuery( function($) {
  mobile = $(window).width();
  if ( mobile <= 480 ){
    jQuery( "div.woocommerce-product-details__short-description" ).insertBefore( ".woocommerce-product-gallery" );
  }
} );
```

### applica css in funzione dello stato del carrello
```js
add_action( 'wp_footer', 'x_hide_cart' );
function x_hide_cart(){
	if ( WC()->cart->get_cart_contents_count() == 0 ) {
		?>
		<style type="text/css">div.top_bar_right{display: none !important;}</style>
		<?php
	}
}
```
