# CNN Bugs and Improvement Suggestions

This document highlights key issues and improvement opportunities identified in the iOS app [CNN: Live & Breaking News](https://apps.apple.com/us/app/cnn-live-breaking-news/id331786748).

## 1 - Deep Linking Does Not Work When Playing Videos

**Severity:** Error  
**Version:** 25.9  
**Build:** 12796264  
**Environment:** Production

Deep linking features generally work well in common scenarios, but when a user is playing a video in full-screen mode, activating a deep link only exits full-screen mode instead of opening the corresponding news article.

### Steps to Reproduce:
1. Save a news link using the share button and paste it elsewhere.
2. Open the app and navigate to a news item that contains a video. Play it in full-screen mode.
3. Tap the saved link. The app will open, but instead of navigating to the linked news article, it will just exit full-screen mode.

<img src="https://github.com/almeidaws/cnniosappanalysis/blob/main/issue1.gif?raw=true" alt="Issue 1" width="320"/>

### Suggestion  
Review the coordinator or routing logic to ensure deep links are handled correctly, even when a video is being played in full-screen mode.

---

## 2 - Gray Bar Appears on Navigation Bar During Refresh

**Severity:** Layout Error  
**Version:** 25.9  
**Build:** 12796264  
**Environment:** Production

When performing the *swipe-to-refresh* gesture, a gray bar briefly appears below the activity indicator. Although subtle, this visual glitch can create a subconscious impression that the app is broken.

### Steps to Reproduce:
1. Open any news article.
2. Swipe down to refresh the page.
3. Observe a brief appearance of a gray bar below the activity indicator.

<img src="https://github.com/almeidaws/cnniosappanalysis/blob/main/issue2.png?raw=true" alt="Issue 2" width="320"/>

### Suggestion  
Investigate the code responsible for rendering the activity indicator on the navigation bar. It may be due to a layout or animation timing issue.

---

## 3 - Text Overlaps with the Switch Component

**Severity:** Layout Adjustment  
**Version:** 25.9  
**Build:** 12796264  
**Environment:** Production

In the notification preferences screen, the label **News about chance encounters, destinations, and vacations** overlaps with the corresponding switch component.

### Steps to Reproduce:
1. Open the **Settings** tab.
2. Tap **Alerts** under **APP PREFERENCES**.
3. Scroll down to the **Travel** section.

<img src="https://github.com/almeidaws/cnniosappanalysis/blob/main/issue3.png?raw=true" alt="Issue 3" width="320"/>

### Suggestion  
Use a `HStack` with appropriate `spacing` to ensure the label and switch do not overlap, following the app's design guidelines.

---

## 4 - "Verification Link Resent" Alert Shows Even After Email Confirmation

**Severity:** Layout/Data Adjustment  
**Version:** 25.9  
**Build:** 12796264  
**Environment:** Production

The message **Verification link resent** continues to be displayed even after the user has confirmed their email. Once confirmed, this message is no longer relevant.

### Steps to Reproduce:
1. Register in the app using a Google account, but do not confirm the email.
2. Open the **Settings** tab.
3. Go to **Account Settings** under **ACCOUNT**.
4. Tap the button to resend the confirmation link.
5. Confirm the email via the received link.
6. Return to the app.
7. Navigate away from and back to the same screen—the message will still be displayed.

<img src="https://github.com/almeidaws/cnniosappanalysis/blob/main/issue4.png?raw=true" alt="Issue 4" width="320"/>

### Suggestion  
Conditionally render the message based on the same logic used to display the verified status (e.g., the green checkmark icon).

---

## 5 - App Does Not Cache Detailed News Pages

**Severity:** Optimization  
**Version:** 25.9  
**Build:** 12796264  
**Environment:** Production

The app seems to refetch news article content every time a user opens the same article, even within a short period. This causes unnecessary network requests, which could be optimized through caching.

### Steps to Reproduce:
1. Go to the **Home** tab and view the list of news articles.
2. Open any article.
3. Close it and reopen the same article shortly afterward.

<img src="https://github.com/almeidaws/cnniosappanalysis/blob/main/issue5.gif?raw=true" alt="Issue 5" width="320"/>

### Suggestion  
Implement caching or use a [HEAD](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/HEAD) request to check for updates before refetching content. This could reduce server load and improve performance.
