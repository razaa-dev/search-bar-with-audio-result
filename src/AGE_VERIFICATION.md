# Age Verification System

## Overview
Para's SFX Library includes a comprehensive age verification system to protect users from accessing NSFW (Not Safe For Work) content without confirming they are 18 years or older.

## How It Works

### 1. **NSFW Tag Detection**
- Any sound tagged with "nsfw" (case-insensitive) is considered mature content
- The system automatically detects NSFW sounds in search results

### 2. **Verification Flow**

#### When Age Verification is Triggered:
- User searches for content that includes NSFW-tagged sounds
- User clicks "View all sounds" when library contains NSFW content
- User clicks on the "NSFW" tag in the Browse by Tag section
- User clicks "All Sounds" tag when NSFW content exists

#### Verification Modal Appears:
1. **Warning Message**: Clear notification about mature content
2. **Age Confirmation**: User must confirm they are 18+
3. **Two Options**:
   - ✅ "Yes, I'm 18+" - Grants access to all content
   - ❌ "No, I'm Under 18" - Filters out NSFW content

### 3. **Content Filtering**

#### If User is NOT Verified:
- NSFW sounds are automatically filtered from search results
- NSFW tag is still visible in Browse by Tag but triggers verification when clicked
- User can access all non-NSFW content normally

#### If User IS Verified:
- All content is accessible
- No filtering applied
- Verification status persists across sessions

### 4. **Storage & Persistence**

#### LocalStorage:
```javascript
{
  verified: boolean,
  timestamp: number
}
```

#### Expiration:
- Verification expires after **30 days**
- User must re-verify after expiration
- No personal data is stored

## Technical Implementation

### Files Created:

1. **`/components/AgeVerification.tsx`**
   - Modal component with responsive design
   - Warning icons and clear messaging
   - Privacy notice included

2. **`/utils/ageVerification.ts`**
   - `isAgeVerified()` - Check verification status
   - `setAgeVerified()` - Store verification
   - `isNSFW()` - Check if sound has NSFW tag
   - `filterNSFWSounds()` - Filter array based on verification
   - `clearAgeVerification()` - Reset verification

### Integration Points:

1. **Search Function**
   - Checks results for NSFW content before displaying
   - Triggers verification modal if needed
   - Filters results if not verified

2. **Tag Browsing**
   - NSFW tag click triggers verification
   - "All Sounds" checks for NSFW content
   - Maintains user experience for non-NSFW searches

3. **View All Sounds**
   - Scans entire library for NSFW content
   - Prompts verification only if NSFW exists
   - Shows filtered results if declined

## User Experience

### Verification Modal Features:
- ⚠️ **Clear Warning Icon** - Red alert triangle with glow effect
- 📝 **Explicit Warning Text** - No ambiguity about content
- 🔒 **Privacy Notice** - Transparency about data storage
- 📱 **Fully Responsive** - Works on all devices
- ⌨️ **Keyboard Accessible** - ESC to close, tab navigation
- 🎨 **Glassmorphism Design** - Matches app aesthetic

### Responsive Design:
- **Mobile**: Compact layout, touch-friendly buttons
- **Tablet**: Medium spacing, easy to read
- **Desktop**: Full-size modal with optimal spacing

## Privacy & Security

### What We Store:
- ✅ Verification status (boolean)
- ✅ Timestamp of verification
- ✅ Stored locally on user's device only

### What We DON'T Store:
- ❌ No personal information
- ❌ No user tracking
- ❌ No server-side storage
- ❌ No cookies
- ❌ No analytics on verification

### Compliance:
- Follows web best practices for age gates
- LocalStorage only - user has full control
- Can be cleared by user at any time
- Transparent about data usage

## Admin Notes

### Adding NSFW Content:
1. Add "nsfw" to the tags field when creating a sound
2. Tag is case-insensitive ("NSFW", "nsfw", "Nsfw" all work)
3. Can be combined with other tags: "nsfw, horror, scream"

### Managing NSFW Tags:
- NSFW tag can be added/removed like any other tag
- Changes take effect immediately
- Content is automatically filtered based on current tags

## Testing Scenarios

### ✅ Test Case 1: First-time User
1. User visits site (not verified)
2. Searches for "nsfw sound"
3. Modal appears
4. User clicks "Yes, I'm 18+"
5. Results show with NSFW content
6. Verification persists on refresh

### ✅ Test Case 2: User Declines
1. User not verified
2. Searches content with NSFW
3. Modal appears
4. User clicks "No, I'm Under 18"
5. Results shown WITHOUT NSFW content
6. Next search doesn't trigger modal (already declined)

### ✅ Test Case 3: Verified User
1. User previously verified
2. All searches show full results
3. No modal interruptions
4. Works across browser sessions

### ✅ Test Case 4: Mixed Content
1. Search returns 10 sounds: 3 NSFW, 7 safe
2. Not verified → Shows 7 safe sounds
3. Verified → Shows all 10 sounds

### ✅ Test Case 5: Tag Navigation
1. Click "NSFW" tag
2. Modal appears if not verified
3. Verify → See NSFW content
4. Decline → See filtered results

## Expiration Handling

After 30 days:
1. Verification automatically expires
2. User re-encounters modal on next NSFW search
3. Must re-verify to access mature content
4. Previous preference is cleared

## Browser Support

Works across all modern browsers:
- ✅ Chrome/Edge (LocalStorage support)
- ✅ Firefox (LocalStorage support)
- ✅ Safari (LocalStorage support)
- ✅ Mobile browsers (iOS/Android)

## Accessibility

- **ARIA Labels**: Proper labeling for screen readers
- **Keyboard Navigation**: Full keyboard support
- **Focus Management**: Proper focus trapping in modal
- **High Contrast**: Warning colors meet WCAG standards
- **Text Size**: Readable at all zoom levels

## Future Enhancements

Potential improvements:
1. **Session-based verification** - Expires when browser closes
2. **Stricter verification** - Additional validation steps
3. **Parental controls** - PIN-based access system
4. **Age gate bypass for admins** - Auto-verify for logged-in admins
5. **Content warnings on individual sounds** - Additional NSFW indicators

## Troubleshooting

### Issue: Modal doesn't appear
- Check if sound has "nsfw" tag (case-insensitive)
- Verify LocalStorage is not disabled
- Check browser console for errors

### Issue: Verification doesn't persist
- Check browser's LocalStorage settings
- Ensure not in incognito/private mode
- Check if LocalStorage quota exceeded

### Issue: Can't access NSFW content
- Verify age confirmation was clicked
- Check if verification expired (30 days)
- Clear LocalStorage and re-verify

## Legal Disclaimer

This age verification system is designed as a basic content filter. It:
- Relies on user honesty
- Provides reasonable protection
- Meets standard industry practices
- Should be combined with proper content moderation
- May not be sufficient for all jurisdictions

**Note**: Administrators should review local laws regarding age verification and adult content distribution.
