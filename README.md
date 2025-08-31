# DrawableBitmap
Convert Image, Shape, Vector  to Bitmap

# From OnCretee Bundle  Pass Drawable Image 
Drawable drawable = ContextCompat.getDrawable(this, R.drawable.my_image);
String base64String = convertDrawableImageToString(drawable);
Log.d("Base64Image", base64String);





# Main Code 

'''
private String convertDrawableImageToString(Drawable drawable){

    if (drawable == null) return null;
    
    // Drawable → Bitmap
    Bitmap bitmap = null;
    if (drawable instanceof BitmapDrawable) {
        bitmap = ((BitmapDrawable) drawable).getBitmap();
    } else {
        // Generic drawable (Vector, Shape, etc.)
        bitmap = Bitmap.createBitmap(drawable.getIntrinsicWidth(),
                drawable.getIntrinsicHeight(), Bitmap.Config.ARGB_8888);
        Canvas canvas = new Canvas(bitmap);
        drawable.setBounds(0, 0, canvas.getWidth(), canvas.getHeight());
        drawable.draw(canvas);
    }

    // Bitmap → ByteArray
    ByteArrayOutputStream baos = new ByteArrayOutputStream();
    bitmap.compress(Bitmap.CompressFormat.PNG, 100, baos);
    byte[] imageBytes = baos.toByteArray();

    // ByteArray → Base64 String
    return Base64.encodeToString(imageBytes, Base64.DEFAULT);
}
'''


# Bitmap তৈরি করার জন্য variable declare

   Bitmap bitmap = null;
