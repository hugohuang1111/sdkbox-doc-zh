## API Reference

### Methods
```lua
sdkbox.PluginReview:setGDPR(enabled)
```
> Set GDPR

```lua
sdkbox.PluginReview:init(jsonconfig)
```
>  initialize the plugin instance.

```lua
sdkbox.PluginReview:setListener(listener)
```
> Set listener to listen for adcolony events

```lua
sdkbox.PluginReview:show(force)
```
> Tells 'SDKBox review plugin' to try and show the prompt (a rating alert).
if you call `show` with `false` or null params,
the prompt will be showed if there is connection available,
the user hasn't declined to rate or hasn't rated current version.
if the item `tryPromptWhenInit` in sdkbox.config is false, you can call this try to show prompt
if you call `show` with `true` params
the prompt will be showed without checks (the prompt is always displayed).
The case where you should call this is if your app has an
explicit "Rate this app" command somewhere. This is similar to rateApp,
but instead of jumping to the review directly, an intermediary prompt is displayed.
another case is for debug

```lua
sdkbox.PluginReview:rate()
```
> goto rating page directly

```lua
sdkbox.PluginReview:userDidSignificantEvent(canPromptForRating)
```

```lua
sdkbox.PluginReview:rateInAppstore(yes)
```

```lua
sdkbox.PluginReview:setCustomPromptTitle(title);
```
> Set customized title for alert view.

```lua
sdkbox.PluginReview:setCustomPromptMessage(message);
```
> Set customized message for alert view.

```lua
sdkbox.PluginReview:setCustomPromptCancelButtonTitle(cancelTitle);
```
> Set customized cancel button title for alert view.

```lua
sdkbox.PluginReview:setCustomPromptRateButtonTitle(rateTitle);
```
> Set customized rate button title for alert view.

```lua
sdkbox.PluginReview:setCustomPromptRateLaterButtonTitle(rateLaterTitle);
```
> Set customized rate later button title for alert view.


### Listeners
```lua
onDisplayAlert()
```
> trigger when alert prompt show

```lua
onDeclineToRate()
```
> trigger when user refuse to rate

```lua
onRate()
```
> trigger when user want to rate

```lua
onRemindLater()
```
> trigger when user want to remind later


