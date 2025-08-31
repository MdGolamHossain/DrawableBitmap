# DrawableBitmap
Convert Image, Shape, Vector  to Bitmap

# From OnCrete Bundle  Pass Drawable Image 
```
Drawable drawable = ContextCompat.getDrawable(this, R.drawable.my_image);
String base64String = convertDrawableImageToString(drawable);
Log.d("Base64Image", base64String);
```




# Main Code 

```
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
```



# Bitmap তৈরি করার জন্য variable declare
```
   Bitmap bitmap = null;
```

Drawable কে Bitmap এ convert করব। Bitmap হলো Android-এ pixel-based image, যা compress এবং byte array-এ convert করা যায়।


#Drawable type check

```
if (drawable instanceof BitmapDrawable) {
    bitmap = ((BitmapDrawable) drawable).getBitmap();
}
```

BitmapDrawable হলো Drawable-এর একটি type যা মূলত already bitmap। যদি drawable ইতিমধ্যে bitmap হয়, তখন সরাসরি bitmap নেওয়া যায়।

কারণ: যদি আমরা আবার Canvas বানাই, এটি unnecessary হবে।


#Non-bitmap drawable handle

```
else {
    bitmap = Bitmap.createBitmap(drawable.getIntrinsicWidth(),
            drawable.getIntrinsicHeight(), Bitmap.Config.ARGB_8888);
    Canvas canvas = new Canvas(bitmap);
    drawable.setBounds(0, 0, canvas.getWidth(), canvas.getHeight());
    drawable.draw(canvas);
}
```


বিভাগে ব্যাখ্যা:

Bitmap.createBitmap(width, height, config)

এটা নতুন bitmap তৈরি করে।

drawable.getIntrinsicWidth() এবং drawable.getIntrinsicHeight() → drawable এর default width/height।

Bitmap.Config.ARGB_8888 → 32-bit color, transparency support করে।

Canvas canvas = new Canvas(bitmap)

Canvas হলো Android-এ drawing surface।

আমরা bitmap এ drawable draw করতে canvas ব্যবহার করি।

drawable.setBounds(0, 0, canvas.getWidth(), canvas.getHeight())

Drawable কে bitmap-এর size অনুযায়ী scale করে।

না দিলে vector বা shape ভুল size এ draw হতে পারে।

drawable.draw(canvas)

Drawable কে actual bitmap এর canvas এ draw করা হলো।

এখন bitmap-এ image ready।

💡 কারণ: VectorDrawable, ShapeDrawable ইত্যাদি bitmap নয়, তাই তাদের bitmap-এ convert করতে হবে।



#Bitmap → ByteArray



```
ByteArrayOutputStream baos = new ByteArrayOutputStream();
bitmap.compress(Bitmap.CompressFormat.PNG, 100, baos);
byte[] imageBytes = baos.toByteArray();
```
ByteArrayOutputStream baos

এটি একটি memory stream যেখানে আমরা bitmap write করব।

bitmap.compress()

Bitmap কে PNG format এ compress করা হলো।

100 → quality (100% best quality)

Output → baos stream

baos.toByteArray()

Stream থেকে actual byte array নিয়ে এলাম।

💡 কারণ: Base64 encode করতে হলে আমাদের byte[] দরকার।



#ByteArray → Base64 String
```
return Base64.encodeToString(imageBytes, Base64.DEFAULT);
```

Base64.encodeToString(byte[], flags) → byte array কে text (string) এ convert করে।

Base64.DEFAULT → default formatting.

এখন তোমার drawable String হিসেবে ready, যা network request বা database-এ পাঠানো যাবে।




























