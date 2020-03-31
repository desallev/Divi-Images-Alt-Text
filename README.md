<h1>fix Divi Alt Text | Get images alt text from media library</h1>

<b>What is the "alt" attribute?</b><br>
The required alt attribute specifies an alternate text for an image, if the image cannot be displayed.
The alt attribute provides alternative information for an image if a user for some reason cannot view it 
(because of slow connection, an error in the src attribute, or if the user uses a screen reader).

<b>Seo - Google</b><br>
In spite of all that search engine robots can do today, they are not - yet - able to read and understand visual formats well. 
They are much more comfortable with text. The "alt" tag was then created to tell the robot what the image contains, in a language 
it understands: it will therefore be able to reference the image.


<b>How to make the alt text work in the divi theme</b><br>
When you upload your image to the image library, include your alt text in the Alt Text box.
If you then add the image to your website using the image module, you must also add the alt text 
at the Advanced Tab, in the Image Alternative Text box. If you don’t add your alt text into 
the image module itself your alt text will not work, even though it’s added at the media library.


/*<br>
Description: Divi Images Alt Text<br>
Author: divimastermind.<br>
Author: https://divimastermind.com/divi-alt-text-in-bildern-fehlt/<br>
*/<br>
<br>
<br>

<b>How to get image alt tags working on a divi website</b><br>
Make a child theme and Insert this "snippet" in the functions.php file.

Enjoy ;)

<pre>
  <code>
    // ALT-Tags aus der Mediathek auslesen
// ========================================================================== //
function get_image_meta( $image, $type = 'alt' ) {
    if ( ! $image ) {
        return '';
    }
 
    $output = '';
 
    if ( '/' === $image[0] ) {
        $post_id = attachment_url_to_postid( home_url() . $image );
    } else {
        $post_id = attachment_url_to_postid( $image );
    }
 
    if ( $post_id && 'title' === $type ) {
        $output = get_post( $post_id )->post_title;
    }
 
    if ( $post_id && 'alt' === $type ) {
        $output = get_post_meta( $post_id, '_wp_attachment_image_alt', true );
    }
 
    return $output;
}
/* Aktualisiert image alt text in Modulen */
function update_module_alt_text( $attrs, $unprocessed_attrs, $slug ) {
    if ( ( $slug === 'et_pb_image' || $slug === 'et_pb_fullwidth_image' ) && '' === $attrs['alt'] ) {
        $attrs['alt'] = get_image_meta( $attrs['src'] );
        $attrs['title_text'] = get_image_meta( $attrs['src'], 'title' );
    } elseif ( $slug === 'et_pb_blurb' && 'off' === $attrs['use_icon'] && '' === $attrs['alt'] ) {
        $attrs['alt'] = get_image_meta( $attrs['image'] );
    } elseif ( $slug === 'et_pb_slide' && '' !== $attrs['image'] && '' === $attrs['image_alt'] ) {
        $attrs['image_alt'] = get_image_meta( $attrs['image'] );
    } elseif ( $slug === 'et_pb_fullwidth_header' ) {
        if ( '' !== $attrs['logo_image_url'] && '' === $attrs['logo_alt_text'] ) {
            $attrs['logo_alt_text'] = get_image_meta( $attrs['logo_image_url'] );
        }
       
        if ( '' !== $attrs['header_image_url'] && '' === $attrs['image_alt_text'] ) {
            $attrs['image_alt_text'] = get_image_meta( $attrs['header_image_url'] );
        }
    }
 
    return $attrs;
}
/* Filter injection */
add_filter( 'et_pb_module_shortcode_attributes', 'update_module_alt_text', 20, 3 );
  </code>
</pre>
