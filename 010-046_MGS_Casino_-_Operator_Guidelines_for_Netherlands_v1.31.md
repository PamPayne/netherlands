---
country: Netherlands
document_name: Operator Guidelines
source_file: 010-046 MGS Casino - Operator Guidelines for Netherlands v1.31.pdf
extracted_date: 2026-04-30
jurisdiction: Netherlands
---

# Operator Guidelines

**Internal**  
**Release Date:** 9th April 2021  
**Document No:** 010-046  
**Version:** 1.3  
**Netherlands**  
**Regulated Market Casino Compliance**

Copyright © Microgaming 2021

## Contents

- Market Overview
- Introduction
- Registration
- Currency
- Languages
- Games
- Bonuses
- Incomplete Games
- Incomplete Games Sweeper
- Player Protection
- Player Balance
- Clock
- Responsible Gaming
- Session Reminders
- Notification Messaging
- In-Game Interface
- External Register Checks
- SAFE

## Market Overview

### Introduction

This document provides an overview for Microgaming Casino operators to comply with the regulations defined in the Netherlands legislation.

It defines the split in responsibilities around managing players and games in the Netherlands market, identifies what Microgaming will provide you with to satisfy the market requirements and assists you in ensuring that the correct integrations are done to satisfy the market requirements.

### Microgaming Solution

The following table summarises the areas and where the responsibilities lie. These responsibilities are defined since Microgaming is not your primary platform and does not contain all the information necessary to manage certain aspects of your players’ life cycle

| Component | Sub Component | Operator | Microgaming |
|---|---|---|---|
| General | Player Verification Services | Yes | No |
| General | Integration – Platform | Yes | No |
| General | Client Legislative Changes | Yes | Yes |
| General | Platform Certification | Yes | No |
| General | License Application and Submission | Yes | Yes |
| Login | Login | Yes | No |
| Login | Online Registration | Yes | No |
| Login | Player Account Management | Yes | No |
| Login | Account Status | Yes | No |
| Login | Last Login Reminder | Yes | No |
| Login | Languages & Currency | Yes | Yes |
| Login | Terms & Conditions and Gaming Contract | Yes | No |
| Lobby | Player Balance | Yes | No |
| Lobby | Player Protection Content | Yes | No |
| Responsible Gaming | (including deposit, limits, exclusions and time-outs) | Yes | No |
| Gaming | RNG Certification | Yes | Yes |
| Gaming | Game Rules | Yes | Yes |
| Gaming | Display Game Rules | Yes | Yes |
| Gaming | Display Game Help | Yes | Yes |
| Gaming | Display RTP | Yes | Yes |
| Back Office | Account Migration | Yes | No |
| Back Office | Game Management | Yes | Yes |
| Back Office | Account Management | Yes | No |
| Back Office | Value Adds | Yes | Yes |
| Back Office | Banking | Yes | No |
| Back Office | Deposit | Yes | No |
| Back Office | Withdrawal | Yes | No |

## Registration

As Microgaming does not have any visibility into your registration process, you would need to manage this aspect as well as the document verification and account activation requirements for your players.

Please ensure that a player is over the age of 18 years to be registered on your casino.

A player may only have one account registered on your casino.

Provisional accounts are prohibited.

Additionally, you must not register an individual as a player until that individual has specified the limits to their gambling behavior. These limits are:

- the maximum duration of access to the player interface per day, week, or month
- the maximum number of deposits to the account per day, week, or month
- the maximum credit in the account

## Currency

To meet regulatory requirements, the Euro will be the currency used for wagers for participation in the games.

## Languages

All game rules and Helpfiles will be provided in Dutch and English by Microgaming.

## Games

The following game changes will be implemented by Microgaming and included in Dutch casinos:

- **Autoplay**
  - AutoPlay is not allowed as per the Netherlands regulations.
  - AutoPlay functionality is not available as a result.
- **Help Files and Game Rules**
  - We shall display the RTP in all help files.
  - Players must be informed of significant changes made to game rules. Please implement this by having the player accept the T&C’s upon login when significant changes occur. Microgaming will inform you of any significant changes via General Notifications as per any updates.
- **Demo Play**
  - Demo play is allowed and will be identical to that of games played for real money.
- **Playcheck**
  - Playcheck will provide data on gaming transactions for a minimum duration of 90 days.

## Bonuses

You may offer bonuses to your players however please ensure that the following conditions are met:

- In game bonuses are prohibited.
- Players must opt in before bonus can be offered.
- Players can revoke bonus eligibility.

## Incomplete Games

Before a player attempts to play a game, they need to be informed of any incomplete games that they may have (i.e. an incomplete games pop up is required).

As Microgaming is not your primary platform and player accounts are owned by yourselves and multiple vendor’s games are offered, we would require you to retrieve any incomplete Microgaming game information for players using the Get Incomplete Games API method. You would need to complete any development required for the user interface, translations, and game redirection.

A valid operator token must be provided in the Authorization header, this can be requested via your Account Manager.

The following request call with mandatory fields needs to be made:

### Request Call

`GET /balances/product/productId/user/userId/incompleteGames`

### Required Fields

| Name | Type | Format |
|---|---|---|
| productid The ProductId (previously called CasinoId or ServerId). This identifies the system where the account is held and can be either a SessionProductId or RegisteredProductId | Integer | int32 |
| userId The UserId of the account. | Integer | int32 |

After a request call is initiated a response will be sent back that will either be a successful or an unsuccessful (error) attempt. If successful, the below information is displayed:

### Responses

| Name | Type | Format |
|---|---|---|
| clientId The ClientId for the game. | Integer | int32 |
| moduleId The ModuleId for the game. | Integer | int32 |
| gameName The name of the game, e.g. HTML5 - Feature Slot - Avalon. | String | int32 |
| moduleName The name of the module, e.g. Feature Slot - Avalon. | String | int32 |
| payouts The combined amount owed to the user, e.g. a payout from a win in a game. | Number | Decimal |
| sessionId SessionId allocated for the session. | Integer | int32 |
| userTransNumber The UserTransNumber is the associated user transaction number for the incomplete game. | Integer | int32 |
| wagers The combined amount taken from the user, e.g. a bet taken during a game. | Number | Decimal |

The /accounts/CheckUserExists API method returns the UserId of the player. You can use this method to retrieve the UserId to do the Imcomplete games call.

### Responses

| Name | Type | Format |
|---|---|---|
| doesExist Indicates whether the specified Username exists or not | Boolean | int32 |
| registeredProductId The ProductId (previously called CasinoId or ServerId) against which the account was created. Only returned if the specified Username already exists. | Integer | int32 |
| suggestedUsernames Array of suggested alternate Username that are currently available. They are based on the provided username. Only returned if the specified Username already exists. | String |  |
| userId The UserID for the account, if the Username already exists | Integer | int32 |

## Incomplete Games Sweeper

We have implemented an Incomplete Games Sweeper.

This will run as a SQL job (FundsInPlayGameSweeper) once a day and will be used to sweep any incomplete game rounds older than the configured threshold.

If a player leaves a bet open for more than the configured threshold the sweeper job cancels the bet and returns the wager for that incomplete game round. The wager will be configured to return the money back to the player.

The job is based on three CasinoID level settings:

- **FundsInPlay - Is Sweeper Enabled:** Used to determine whether the sweeper should even be used for this serverid.
- **FundsInPlay - InActivity period in days:** Used to determine the threshold for how old a game round needs to be before sweeping it.
- **FundsInPlay-Sweep to OpOrPlayer 0-Op 1-Player:** Used to determine whether the money is returned to the player or not. Default will be 1 (Sweep to Player)

**Note:**  
You are required to advise your players regarding this on your Terms and Conditions page.

## Player Protection

Player protection information must be accessible to players on the casino website at all times.

Since you manage the lobby for your casino, please ensure that you provide players with the following info:

- the initial balance in the account on the last login
- the total stake since the last login
- the total winnings and the total losses since the last login

Additionally, the last date and time of the player’s previous login must be displayed upon the player login on your website.

## Player Balance

As Microgaming is not your primary platform and player accounts are owned by yourselves, you would be required to continually show the player’s balance during the entire time the player has logged into the system.

## Clock

You must display a clock that allows the player to continuously see what time it is in the Netherlands.

## Responsible Gaming

Responsible gaming measures need to be in place to ensure that all players play responsibly and are in control of managing their gaming activity. Since Microgaming operators own the player accounts and manage their own lobby, you must ensure that the following responsible gaming options are accessible to your players:

- **Limits**
  - Upon registration, players are required to set a deposit limit and a session limit over daily, weekly, or monthly periods.
  - Upon registration, players must also set a maximum level of credit that can be held in their account.
  - Limit reductions are required to be implemented immediately.
  - Limit increases are subject to a 7-day cooling off period.
- **Self-Exclusions**
  - An internal exclusion system must exist that allows the player to self-exclude.
  - Players who self-exclude or are self-excluded externally, cannot participate in games or deposit.
  - A player who self-excludes may revoke that exclusion after 6 months.
- **Error Codes**
  - To enable the Microgaming system to display informative error messages to players, we require that the following error codes are returned under the prescribed conditions.

| Code | Description |
|---|---|
| 6102 | Account is locked. |
| 6104 | Player is self-excluded. |
| 6109 | Self-exclusion is over and the player must contact the operator to lift the exclusion. The cooling period is effective once this is done. |
| 6110 | Self-exclusion is over, but the player is in the cooling period. |
| 6112 | Player is deactivated. |
| 6113 | Player is self-excluded on registry |
| 6503 | Player has insufficient funds. |
| 6505 | The player exceeded their daily protection limit. |
| 6506 | The player exceeded their weekly protection limit. |
| 6507 | The player exceeded their monthly protection limit. |
| 6508 | The player exceeded their game play duration. |
| 6509 | The player exceeded their loss limit. |
| 6510 | The player is not permitted to play this game. |
| 6114 | The player has exceeded their daily login duration limit. |
| 6115 | The player has exceeded their weekly login duration limit. |
| 6116 | The player has exceeded their monthly login duration limit. |

## Session Reminders

Session reminders must be included as a reality check, to assist players in keeping track of how long they have been logged in. You must ensure that:

- The duration (time elapsed) of how long the player has been logged in, is continuously displayed.
- The player cannot continue their game or start a new game if the session limit has been reached. When it is reached, the player must be informed. The game (spin) can be completed but the session must be terminated once that game is complete. If the player starts a new session, they must not be allowed to continue playing.

Microgaming has 2 options for you to choose from namely Notification Messaging and In-Game Interface:

### Notification Messaging

The ability to notify the player has been implemented in HTML5, this allows information to be displayed to the player. The notification will require a player interaction to close the message. The notification will be displayed to the player over the game with or without blocking the player from interacting with the game, based on the modal setting chosen by the Operator. It can contain a hyperlink using the encoded HTML (eg: anchor / href tags) to configure it. This hyperlink will redirect the page on mobile, or launch in a new window on desktop.

The play method data request is constructed as follows (as it is now, no changes are required):

#### Request:

```xml
<pkt>
<methodcall name="play" system="casino" timestamp="2018/07/10 08:29:43.736">
<auth login="ThirdParty_User" password="******"/>
<call actiondesc="" actionid="9000000840" amount="225" currency="USD" finish="false" gameid="37"
gamereference="MGS_HTML5_Slot_Thunderstruck" offline="false" playtype="bet" seq="ede2f82d-4ca5-4d8b-
a0c8-c7f6077eb82c" start="true" token="0602f4b1-509e-4920-9a98-f82b96fc0828">
<extinfo/>
</call>
</methodcall>
</pkt>
```

A new optional element named message should be added to the result element on the play bet response. Attributes are:

- **Mode** – possible values are ‘modal’ and ‘nonmodal’.
  - If ‘modal’ is supplied, our game should prevent further gameplay, provided the current game round is completed and the client is not in a busy state.
  - If ‘nonmodal’ is supplied, our game should allow reels to continue spinning, provided the current game round is completed and the client is not in a busy state.
- **Contents** – this should be an html encoded message that our game will display to the player. Max length is 512 characters.

The data response is constructed as follows (data example without a hyperlink):

#### Response:

```xml
<pkt>
<methodresponse name="play" timestamp="2018/07/10 08:29:43.741">
<result balance="21525" bonusbalance="0" exttransactionid="525715622" seq="ede2f82d-
4ca5-4d8b-a0c8-c7f6077eb82c" token="84783adc-802f-49f0-b62c-37fb22c16c4c">
<message contents="&lt;h1&gt;This is an examples 1
st 
line of text.&lt;/h1&gt;
&lt;p&gt;This is an example of 2
nd 
line of text. &lt;/p&gt;" mode="nonmodal" />
<extinfo/>
</result>
</methodresponse>
</pkt>
```

The data response is constructed as follows (data example with a hyperlink):

#### Response:

```xml
<pkt>
<methodresponse name="play" timestamp="2018/07/10 08:29:43.741">
<result balance="21525" bonusbalance="0" exttransactionid="525715622" seq="ede2f82d-
4ca5-4d8b-a0c8-c7f6077eb82c" token="84783adc-802f-49f0-b62c-37fb22c16c4c">
<message contents="&lt;p&gt;This is an example message click &lt;a
href=&quot;http://microgaming.co.uk&quot;&gt;here&lt;/a&gt; for more&lt;/p&gt;" mode="modal"/>
<extinfo/>
</result>
</methodresponse>
</pkt>
```

### In-Game Interface

The In-Game Interface has been extended to allow operators to call showNotification, passing in the contents of the message. The method is used is:

`mgs.inGameInterface.showNotification(data);`

The data object is constructed as follows:

#### Data Example

```javascript
var data = {
notificationid: 'bonusfundsnotification',
title: 'Title Text',
message: 'Message Text with {hyperlink}',
secondstilldismiss: 30,
url: 'https://termsandconditions.com'
};
```

| Key | Description | Type | Static Value |
|---|---|---|---|
| notificationid | The id of the notification. This will be used for tracking/metrics. This must not be changed | String | Bonusfundsnotification |
| title | The notification title. E.g. 'Bonus Funds' | String |  |
| message | The notification message. If a hyperlink is required in the message, the hyperlink text should be wrapped in { }. The message should not contain any specific formatting tags. E.g 'This is my message, {click here} to launch the link' | String |  |
| secondstilldismiss | Time until the notification automatically dismisses. If set to 0, the notification will only be dismissed by user interaction | Number |  |
| url | The url is required if the message contains a hyperlink. This is launched when the user selects the hyperlink text. This must be the full url including http(s). E.g. ' ' | String |  |

## External Register Checks

A central register system across all game forms shall be maintained to protect players and combat addiction to games of chance. MGS Casino has no visibility of your players prior to launching a MGS game.

As such, please ensure that your players are not on the central exclusion register. Any player on the central register system, must not be allowed to launch any game. Please ensure that any player on the central register system, be excluded from all brands for your casino.

## SAFE

SAFE refers to the data storage system (control database), where the licence holder must store data for all games executed by the licence holder. The SAFE server must be hosted within the Netherlands.

As Microgaming is not your primary platform and multiple vendor’s games are offered, please ensure that you configure a SAFE environment, in order to:

a) Enable the regulator to access, monitor and supervise any of the components of the technical gaming platform.  
b) Control game security and transparency.  
c) Provide reports as required by the regulator.

Please ensure that player activity data is kept for a minimum of 3 years. The total amount that each player has wagered, however, must be kept for 7 years.