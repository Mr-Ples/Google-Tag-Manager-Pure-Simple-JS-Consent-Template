# Google Tag Manager Consent Template using Pure Simple JS + HTML
Pure JS + HTML simple consent implementation:
1) Denies all non-essential cookies by default. 
2) Checks `localStorage` itself on start for consent and grants consent if already given.
3) Grant consent with `localStorage` and `window` objects in your cookie banner code.

Heavily simplified version of template found on Google  help (see https://developers.google.com/tag-platform/tag-manager/templates/consent-apis).

## Setup
### 1) Setup Consent Mode
- Enable Consent Mode in Google Tag Manager
  - `Admin > Container Settings > Additional Settings > Enable Consent Mode Overview > Save`
- Bulk edit all tags to require consent
  - `Tags > Shield Icon (next to New button) > Select all > Shield Icon (with gear next to triple dots) > Require additional consent for tag to fire > Add required consent for each: analytics_storage, ad_user_data, ad_personalization, ad_storage > Save`

### 2) Import template
- Import the `.tpl` file into Google Tag Manager
  - `Templates > New > Import (top right)`
- Edit the `localStorage` variable name you would be using in your code
  - `Permissions > Access local storage > Edit key`
  - *Remember to change the key name in the code tab as well wherever `localStorage` is used
- Add a new tag using the new template
  - `Tags > New > Tag Configuration > Custom > Simple Consent JS + HTML`
- Add Consent Initilization trigger
  - `Triggering > Consent Initilization`

### 3) Add window object to html
- Add the following snippet above where Google Tag Manager snippet appears in your code:
```js
window.consentListeners = [];
window.addConsentListener = (callback) => {
  window.consentListeners.push(callback);
};
```

### 4) Change consent when user taps your custom cookie banner accept button:
*Replace `COOKIES_CONSENT` with the name you used above in the template for local storage permissions
```js
function onAcceptClick() {
  localStorage.setItem(COOKIES_CONSENT, "true");
  window?.consentListeners?.forEach((callback: () => void) => {
    callback();
  });
}
```
