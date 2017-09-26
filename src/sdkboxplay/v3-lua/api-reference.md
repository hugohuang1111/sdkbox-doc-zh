## API Reference

### Methods
```lua
sdkbox.SdkboxPlay:init ( ) ;
```
> Initialize the plugin instance.
> The plugin initializes from the sdkbox_config.json file, and reads a configuration of the form:
> {
>     "leaderboards"     : LeaderboardObject[],
>     "achievements"     : AchievementObject[],
>     "connect_on_start" : boolean,
>     "debug"            : boolean,
>     "enabled"          : boolean
> }
> debug:
>    is a common value to all plugins which enables debug info to be sent to the console. Useful when developing.
> enabled:
>    is a common value to all plugins, which enables or disables the plugin. If enabled is false, the plugin methods > will do nothing.
> connect_on_start:
>    tells the plugin to make an automatic connection to Google Play Services on application startup.
> leaderboards:
>    a collection of objects of the form:
>    {
>        "id"   : // google play's assigned leaderboard id
>        "name" : // human readable leaderboard name. You'll request leaderboard actions with this name.
>    }
> achievements:
>    a collection of objects of the form:
>    {
>        "id"          : // google play's assigned achievement id.
>        "name"        : // human readable achievement name. You'll request achievement actions with this name.
>        "incremental" : // boolean
>    }

```lua
sdkbox.SdkboxPlay:setListener ( listener ) ;
```
> Set SdkboxPlay plugin listener.

```lua
sdkbox.SdkboxPlay:submitScore( leaderboard_name, score );
```
> Request submission of an score value to a leaderboard name defined in sdkbox_config.json file.
> If the leaderboard name does not exists, or the id associated is not defined in the Developer Console for the application, the call will silently fail.
> If everything's right, it will notify the method <code>onScoreSubmitted</code>.

```lua
sdkbox.SdkboxPlay:showLeaderboard( leaderboard_name );
```
> Request to show the default Leaderboard view.
> In this view you'll be able to interactively select between daily, weekly or all-time leaderboard time frames and the scope to global or you google play's friends results.

```lua
sdkbox.SdkboxPlay:unlockAchievement( achievement_name );
```
> Request to unlock an achievement defined by its name.
> This method assumes the achievement is non incremental.
> If the achievement type is incorrectly defined in the configuration file, or the play services determines it is of the wrong type, this method will fail silently.
> Otherwise, if everything is right, the method <code>onAchievementUnlocked</code> will be invoked on the plugin listener.

```lua
sdkbox.SdkboxPlay:incrementAchievement( achievement_name, increment );
```
> Request to increment the step count of an incremental achievement by the specified number of steps.
> This method assumes the achievement is incremental.
> If the achievement type is incorrectly defined in the configuration file, or the play services determines it is of the wrong type, this method will fail silently.
> If the call is successful, this method may invoke two different methods:
>  + <code>onIncrementalAchievementStep</code> if the achievement is not unlocked.
>  + <code>onIncrementalAchievementUnlocked</code> the first time it's been newly unlocked.

```lua
sdkbox.SdkboxPlay:showAchievements( );
```
> Request to show the default Achievements view.
> In this view, you'll only see public achievements.
> It will show wether or not achievements are unlocked, and the steps towards unlocking it for incremental achievements.
> Total experience count is measured as well.

```lua
sdkbox.SdkboxPlay:isConnected();
```
> Fast method to know plugin's connection status to the play services.

```lua
sdkbox.SdkboxPlay:signin();
```
> Request connection to the platform-specific services backend.
> This method will invoke plugin's listener <code>onConnectionStatusChanged</code> method.

```lua
sdkbox.SdkboxPlay:signout();
```
> Request disconnection from the GooglePlay/Game Center backend.
> This method will invoke plugin's listener <code>onConnectionStatusChanged</code> method.

```lua
sdkbox.PluginSdkboxPlay:loadAllData()
```
> **DEPRECATED**
Please use loadAllGameData to replace, load all saved user game data in clound, will trigger onGameData callback

```lua
sdkbox.PluginSdkboxPlay:loadGameData(save_name)
```
> **DEPRECATED**
Please use loadAllGameData to replace, load one saved user game data in clound, will trigger onGameData callback

```lua
sdkbox.PluginSdkboxPlay:saveGameData(save_name, data)
```
> **DEPRECATED**

Please use saveGameDataBinary(name, data, length) to replace, save user game data in cloud, will trigger onGameData callback

```lua
sdkbox.PluginSdkboxPlay:loadAllGameData()
```
> load all saved game data
will trigger onLoadGameData callback

```lua
sdkbox.PluginSdkboxPlay:saveGameDataBinary(name, data, length)
```
> save user game data, will trigger onSaveGameData callback

- @param name: saved data name
- @param data: data pointer
- @param length: data length in byte


### Listeners
```lua
onConnectionStatusChanged( status );
```
> Call method invoked when the Plugin connection changes its status.
> Values are as follows:
>   + 1000:     successfully connected.
>   + 1001:     successfully disconnected.
>   + 1002:     error with google play services connection.

```lua
onScoreSubmitted(
        leaderboard_name,
        score,
        maxScoreAllTime,
        maxScoreWeek,
        maxScoreToday )
```
> Callback method invoked when an score has been successfully submitted to a leaderboard.
> It notifies back with the leaderboard_name (not id, see the sdkbox_config.json file) and the
> subbmited score, as well as whether the score is the daily, weekly, or all time best score.
> Since Game center can't determine if submitted score is maximum, it will send the max score flags as false.

```lua
onIncrementalAchievementUnlocked( achievement_name );
```
> Callback method invoked when the request call to increment an achievement is succeessful and that achievement gets unlocked. This happens when the incremental step count reaches its maximum value.
> Maximum step count for an incremental achievement is defined in the google play developer console.

```lua
onIncrementalAchievementStep( achievement_name, step );
```
> Callback method invoked when the request call to increment an achievement is successful.
> If possible (Google play only) it notifies back with the current achievement step count.

```lua
onAchievementUnlocked( achievement_name, newlyUnlocked );
```
> Call method invoked when the request call to unlock a non-incremental achievement is successful.
> If this is the first time the achievement is unlocked, newUnlocked will be true.

```lua
onGameData(action, name, data, error)
```
> **DEPRECATED**

- @param action string save, load
- @param name string
- @param data string
- @param error string if load/save success, error will be empty

```lua
onSaveGameData(success, error)
```
>
- @param success bool
- @param error string if success, error will be empty

```lua
onLoadGameData(savedData, error)
```
>
- @param savedData SavedGameData
- @param error string if success, error will be empty

