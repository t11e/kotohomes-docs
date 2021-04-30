# Kotohomes Widget API

## Introduction

Widgets are small embeddable views that can be put on a web page and display information from the real-estate application.

## Example

```html

    <link rel="stylesheet" href="https://widgets.kotohomes.com/widgets.css" />
    <script src="https://widgets.kotohomes.com/widgets.js" charset="utf-8" async></script>

    <script>
      window._kotohomesWidget = window._kotohomesWidget || [];
      if (typeof window.kotohomesWidget === 'undefined') {
        window.kotohomesWidget = function() {
          _kotohomesWidget.push(arguments);
        };
      }
    </script>

    <div id="widget" class="widget_container"></div>

    <script>
      kotohomesWidget({
        siteId: 'site_YmivKwyCN88E309HgWyLZF4AxJg', // Get this from us
        type: 'listings',
        target: document.getElementById('widget'),
        options: {
          property_type: 'residential',
          status: 'active'
        }
      });
    </script>
```

### `kotohomesWidget({siteId, type, target, options: {}})`

Embeds a widget in a DOM element. The document does not have to be ready for this method to be called. `listings` is currently the only type of widget supported.

The target element must be true DOM element.

### `listings` Widget

The listings widget renders one or more listings in a compact grid.

#### Options

`status` (string or array of strings) - if present, filter to include only these status. Default is `active`

`new_listings_only` (boolean) - only include new listings within 14 days (Default: false).

`show_agent` (boolean) - show associated agent & broker next to the listing (Default: false).

`result_limit` (integer) — number of results to show (Default: 1).

`tiles_per_row` (integer) - number of tiles to show per row.

`mls_number` (string) - display listing with associated mls number.

`bounding_area` (string, e.g. `[[lng, lat], [lng, lat]]` where lat/lng are decimals) - filters results to the bounding area.

`agent_uid` (string, e.g. `post.agent:endeavor.enclosure.sitename$1234` or array of strings) - filters results associated with agent.

`broker_uid` (string, e.g. `post.broker:endeavor.enclosure.sitename$1234` or array of strings) - filters results associated with broker.

`property_type` (enum, see below, or an array of enums) - filters results by property_type.

    commercial lease
    commercial sale
    farm
    lots_land
    multifamily
    other
    residential
    residential common interest
    residential lease
    unspecified

`zip_codes` (string or array of strings) - filters results by zip_code.
