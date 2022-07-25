# Enclosurehomes Advanced Ad Integration

We require a single function for placing ads on the page which is expected to return a cleanup function that
can be used to remove them as necessary.

This is typically wrapped in a command queue pattern to allow us to call your function after your custom script has
fully loaded.

## The `createAd` Function

The `createAd` function that you'll implement takes the following arguments:

* `domId` - ID of the element in which to place the ad
* `adName` - Name of the ad slot, you can use this to map ads to your own internal positions.
* `targeting` - Dictionary of any available ad targeting key/value pairs.

See below for a description of the available slots and targeting key/value pairs.

Here is a template that can be filled in:

```javascript
function createAd(domId, adName, targeting) {
    // TODO create and place the ad on the page
    
    return function () {
        // TODO remove the ad from the page        
    }
}
```

The removal function should do nothing if the ad was never fully created (say you used `googletag.cmd.push` during
creation) and both create/remove should gracefully do nothing if the DOM element is no longer available.

## Queue Wrapping Pattern

Because we allow you to inject your own Javascript code into the page we can't call your `createAd` function directly
as it might not yet have loaded and be available. You will need to supply a safe way for us to call your function, the
command queue pattern used by Google Ads works well for this.

Following is a walk through with the name `Acme Ads` to avoid conflicts on the `window` object. You should use your own
company name here.

1. Supply us with your custom script URL, this must be served over HTTPS. e.g.:

    ```
    https://example.com/js/acme_ads.min.js
    ```

2. Document your queueing behavior so we can write a driver for your ad serving logic. e.g.:

    ```javascript
    window.acme_ads = window.acme_ads || {cmd: []}
    ```

   Which we could call like so:

    ```javascript
    let removeAd
    window.acme_ads.cmd.push(function (createAd) {
        // our code that calls your createAd function goes here, e.g.:
        removeAd = createAd('exampleAdElementId', 'header_leaderboard', {})
    });
   
    // sometime later our page will call the returned cleanup function:
    if (removeAd) {
        removeAd()
    }
    ```

   You are responsible for replacing `window.acme_ads.cmd` with your queue processing logic while keeping track
   of any previously queued ad calls.

   The `createAd` function you've defined should be passed to the queued functions as you call them so our code can
   call your `createAd` function.

## Available Ad Slots

| Ad Name                       | Size    | Location                                                  |
|-------------------------------|---------|-----------------------------------------------------------|
| header_leaderboard            | 728x90  | top of all pages                                          |
| search_results_leaderboard    | 728x90  | interleaved row on the search results page                |
| search_results_tile           | 300x250 | interleaved tile on the search results page               |
| listing_detail_tile           | 300x250 | bottom of the listing detail page                         |

## Available Ad Targeting Key/Value Pairs

All targeting data is optional and won't be in the dictionary when unavailable for a page.

| Key        | Page           | Description                          | Example Value  |
|------------|----------------|--------------------------------------|----------------|
| zip_code   | listing detail | ZIP code of the current listing      | `10001`        |
| zip_code   | search         | ZIP code of the current search       | `10001`        |
| territory  | search         | Territory id of the current search   | `my_territory` |
