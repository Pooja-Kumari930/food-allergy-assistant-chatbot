## QUICK TEST GUIDE - Food Allergy Assistant

### Before Testing
- Open Chrome/Firefox/Edge DevTools (F12)
- Go to Console tab to see [v0] debug logs
- Ensure allergies are selected before testing

---

## TEST 1: BUTTON STYLING (30 seconds)

**Step 1:** Open the app
**Expected:** See 4 colored buttons at bottom:
- Voice: Purple
- Text: Blue  
- Barcode: Indigo
- Image: Emerald/Green

**Step 2:** Hover over each button
**Expected:** Colors brighten (darker shade on hover)

**Result:** ✓ PASS if all 4 buttons have solid colors with white text

---

## TEST 2: TEXT INPUT - ALLERGEN DETECTION (1 min)

**Setup:**
1. Click "Text" button
2. Select "milk" allergy
3. Type in input: "butter, cream, salt"
4. Press Enter or click "Check"

**Console should show:**
```
[v0] Text input submitted via Enter key
[v0] Starting food check with ingredients: butter, cream, salt
[v0] Analyzing ingredients...
[v0] Found allergen match: milk via keyword: butter
[v0] Found allergen match: milk via keyword: cream
```

**Chat should show:**
```
You: butter, cream, salt
Assistant: ⚠️ HIGH RISK - ALLERGEN DETECTED!
I found the following allergens:
• milk
```

**Then:**
1. Input field should be empty
2. Cursor should be in input field (focused)
3. Type another message - should work immediately

**Result:** ✓ PASS if allergen detected AND input is reusable

---

## TEST 3: BARCODE LOOKUP (2 min)

**Setup:**
1. Click "Barcode" button
2. Select "water, sugar, coffee" as ingredients to test
3. Select any allergy (e.g., "milk")

**Method A - Manual Entry:**
1. Click "Enter Barcode Manually"
2. Type: `737628064502`
3. Click "Check Barcode"

**Console should show:**
```
[v0] Barcode lookup requested for: 737628064502
[v0] Calling Open Food Facts API: https://world.openfoodfacts.org/api/v0/product/737628064502.json
[v0] API response received, status: 1
[v0] Product found: Coca-Cola
[v0] Using ingredients_text
[v0] Extracted ingredients: carbonated water, high fructose corn syrup...
```

**Chat should show:**
```
📷 Scanned barcode: 737628064502

📦 Product: Coca-Cola
✓ SAFE!
Based on your allergies (milk), these ingredients appear safe:
• carbonated water
• high fructose corn syrup
```

**Result:** ✓ PASS if product name and ingredients extracted

---

## TEST 4: IMAGE UPLOAD - OCR + CLASSIFICATION (3 min)

**Setup:**
1. Click "Image" button
2. Select "milk" allergy

**Test A - Image with Text Label:**
1. Upload image of milk carton
2. System should extract text via OCR (Tesseract)
3. Should detect "milk" and show allergen warning

**Console should show:**
```
[v0] Starting OCR processing
[v0] Tesseract.js imported successfully
[v0] OCR Progress: recognizing 45%
[v0] OCR complete, extracted text length: 245
[v0] Cleaned text: milk, vitamin d, lactose...
```

**Chat should show:**
```
🖼️ Image analysis: milk, vitamin d, lactose...
⚠️ HIGH RISK - ALLERGEN DETECTED!
I found the following allergens:
• milk
```

**Test B - Image without Text (e.g., fish photo):**
1. Upload clear fish image
2. If OCR fails → Falls back to MobileNet classification
3. Should detect food visually

**Console should show:**
```
[v0] OCR text insufficient, attempting image classification...
[v0] Loading MobileNet model...
[v0] Classification complete: [fish (0.92), salmon (0.85)]
[v0] Image analysis complete: {method: 'visual', detectedFoods: ['fish']}
```

**Chat should show:**
```
🖼️ Image analysis: fish
⚠️ HIGH RISK - ALLERGEN DETECTED!
Contains fish
```

**Result:** ✓ PASS if OCR or visual detection works

---

## TEST 5: VOICE INPUT (2 min)

**Setup:**
1. Click "Voice" button
2. Select "peanuts" allergy
3. Allow microphone access when prompted

**Steps:**
1. Click microphone button
2. Say: "I want to eat peanut butter"
3. Wait for recognition to complete

**Console should show:**
```
[v0] Speech recognition started
[v0] Transcript: I want to eat peanut butter
[v0] Speech recognition complete
[v0] Starting food check with ingredients: I want to eat peanut butter
[v0] Found allergen match: peanuts via keyword: peanut
```

**Chat should show:**
```
You: I want to eat peanut butter
Assistant: ⚠️ HIGH RISK - ALLERGEN DETECTED!
Contains peanuts
```

**Result:** ✓ PASS if speech recognized and allergen detected

---

## TEST 6: GARBAGE TEXT FILTERING (1 min)

**Setup:**
1. Click "Text" button
2. Type: "qpad, xyz, water, salt"
3. Press Enter

**Console should show:**
```
[v0] Cleaning OCR text...
[v0] Filtering garbage word: qpad (no vowels)
[v0] Filtering garbage word: xyz (only consonants)
[v0] Cleaned ingredients: [water, salt]
```

**Chat should show:**
```
✓ SAFE!
Based on your allergies, these ingredients appear safe:
• water
• salt
```

**Result:** ✓ PASS if garbage filtered and only valid words processed

---

## TEST 7: CONTINUOUS CHAT (1 min)

**Setup:**
1. Click "Text" button
2. Type: "milk"
3. Check result

**Then immediately:**
1. Type: "eggs"
2. Check result
3. Type: "water"
4. Check result

**Expected:** All 3 messages send and receive responses without any issues

**Console should show:**
```
[v0] Text input submitted via Enter key (appears 3 times)
[v0] Starting food check... (appears 3 times)
```

**Chat should show:**
```
You: milk
Assistant: ⚠️ ALLERGEN DETECTED - milk

You: eggs
Assistant: ⚠️ ALLERGEN DETECTED - eggs

You: water
Assistant: ✓ SAFE!
```

**Result:** ✓ PASS if all 3 messages processed without blocking

---

## COMPREHENSIVE TEST CHECKLIST

### UI/Styling
- [ ] Voice button purple
- [ ] Text button blue
- [ ] Barcode button indigo
- [ ] Image button emerald
- [ ] All buttons have white text
- [ ] Buttons change color on hover
- [ ] All buttons clickable

### Allergen Detection
- [ ] "milk" with milk allergy → Detected
- [ ] "butter, cream" with milk allergy → Both detected as milk variants
- [ ] "qpad, xyz" → Filtered as garbage
- [ ] "water, salt" → Shows SAFE
- [ ] Multiple allergens detected in single message

### Barcode
- [ ] Valid barcode (737628064502) returns product
- [ ] Invalid barcode shows "not found"
- [ ] Ingredients extracted correctly
- [ ] API calls logged with [v0] prefix

### Image
- [ ] OCR extracts label text
- [ ] MobileNet classifies food visually
- [ ] Garbage text filtered
- [ ] Allergens detected from image

### Voice
- [ ] Microphone button works
- [ ] Speech recognized
- [ ] Transcript shown in chat
- [ ] Allergen detected from voice input

### Chat
- [ ] Input clears after send
- [ ] Focus returns to input
- [ ] Can send unlimited messages
- [ ] No duplicate messages
- [ ] Chat history preserved

---

## DEBUGGING TIPS

**If buttons are dark:**
- Check browser console for CSS errors
- Verify Tailwind classes are in HTML
- Hard refresh (Ctrl+Shift+R)

**If allergen not detected:**
- Check [v0] logs in console
- Verify allergen is in dictionary
- Try exact match first

**If barcode returns nothing:**
- Check Network tab for API call
- Verify barcode number is valid
- Open Food Facts may not have all products

**If image too slow:**
- First load loads ML model (5-7s)
- Subsequent loads cached (2-3s)
- Check if browser is blocking downloads

**If voice not working:**
- Check microphone permissions
- Try different browser
- Check if HTTPS is enabled

---

## SUCCESS CRITERIA - ALL MUST PASS ✓

- [ ] All 4 buttons visible with correct colors
- [ ] Text input works repeatedly without blocking
- [ ] Barcode 737628064502 returns Coca-Cola
- [ ] Image with milk detected triggers allergen warning
- [ ] Voice input captured and processed
- [ ] Garbage text "qpad" filtered automatically
- [ ] Chat history maintained across messages
- [ ] All [v0] logs appear in console
- [ ] No errors in browser console
- [ ] UI feels smooth and responsive

**If all checks pass: SYSTEM IS PRODUCTION READY ✅**
