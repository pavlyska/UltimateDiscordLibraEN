# Ultimate Discord Libra  
> **Ultimate Discord Libra** — this is a comprehensive list of all known libraries for interacting with the Discord API in **all possible programming languages**, including obsolete and obscure ones.  
> In this project, we will show:
> - A comparison of all available libraries
> - The best library choice for each programming language
> - Implementation of a simple `/ping` command that replies with `Pong!` using each library

---
## 📚 Table of Contents  
1. [JavaScript (Node.js)](#javascript-nodejs)  
2. [Python](#python)  
3. [Java](#java)  
4. [C# (.NET)](#c-net)  
5. [Go (Golang)](#go-golang)  
6. [Rust](#rust)  
7. [Ruby](#ruby)  
8. [PHP](#php)  
9. [TypeScript](#typescript)  
10. [Dart / Flutter](#dart--flutter)  
11. [Swift (iOS)](#swift-ios)  
12. [Kotlin (Android)](#kotlin-android)  
13. [C++](#c)  
14. [Lua](#lua)  
15. [Perl](#perl)  
16. [R](#r)  
17. [Elixir](#elixir)  
18. [Clojure](#clojure)  
19. [Haskell](#haskell)  
20. [Scala](#scala)  
21. [F#](#f)  
22. [Groovy](#groovy)  
23. [Shell / Bash](#shell--bash)  
24. [VB.NET](#vbnet)  
25. [Objective-C](#objective-c)  
26. [Julia](#julia)  
27. [MATLAB](#matlab)  
28. [COBOL](#cobol)  
29. [Fortran](#fortran)  
30. [Assembly](#assembly)  
---
## JavaScript (Node.js)  
### 1. discord.js (v14) – *best*  
```js
const { Client, GatewayIntentBits } = require('discord.js');
const client = new Client({ intents: [] });
client.on('interactionCreate', async interaction => {
    if (!interaction.isChatInputCommand()) return;
    if (interaction.commandName === 'ping') {
        await interaction.reply('Pong!');
    }
});
client.login('TOKEN');
// Command registration via REST API separately
```
### 2. eris  
```js
const Eris = require("eris");
const bot = new Eris.Client("TOKEN");
bot.on("interactionCreate", (interaction) => {
    if (interaction.data?.name === "ping") {
        bot.createMessage(interaction.channel.id, "Pong!");
    }
});
bot.connect();
```
### 3. yui.js  
```js
const Yui = require('yui.js');
const bot = new Yui.Client('TOKEN');
bot.slashCommands.set('ping', {
    description: 'Sends Pong!',
    run: (ctx) => ctx.reply('Pong!')
});
```
### 4. discord-harmony (deprecated)  
```js
const Harmony = require('discord-harmony');
const bot = new Harmony.Client('TOKEN');
bot.onSlash('ping', () => 'Pong!');
bot.login();
```
---
## Python  
### 1. discord.py – *best*  
```python
from discord.ext import commands  
bot = commands.Bot(command_prefix="!")  
@bot.slash_command(name="ping", description="Sends Pong!")  
async def ping(ctx):  
    await ctx.respond("Pong!")  
bot.run("TOKEN")
```
### 2. nextcord  
```python
import nextcord  
from nextcord.ext import commands  
bot = commands.Bot()  
@bot.slash_command(name="ping", description="Sends Pong!")  
async def ping(interaction: nextcord.Interaction):  
    await interaction.response.send_message("Pong!")  
bot.run("TOKEN")
```
### 3. py-cord  
```python
import discord  
from discord.commands import Option  
bot = discord.Bot()  
@bot.slash_command(name="ping", description="Sends Pong!")  
async def ping(ctx):  
    await ctx.respond("Pong!")  
bot.run("TOKEN")
```
### 4. disnake  
```python
import disnake  
from disnake.ext import commands  
bot = commands.Bot()  
@bot.slash_command(name="ping", description="Sends Pong!")  
async def ping(inter: disnake.ApplicationCommandInteraction):  
    await inter.response.send_message("Pong!")  
bot.run("TOKEN")
```
---
## Java  
### 1. JDA – *best*  
```java
import net.dv8tion.jda.api.JDABuilder;  
import net.dv8tion.jda.api.events.interaction.command.SlashCommandInteractionEvent;  
import net.dv8tion.jda.api.hooks.ListenerAdapter;  
public class PingBot extends ListenerAdapter {  
    public static void main(String[] args) throws Exception {  
        JDABuilder.createDefault("TOKEN")  
                .addEventListeners(new PingBot())  
                .build();  
    }  
    @Override  
    public void onSlashCommandInteraction(SlashCommandInteractionEvent event) {  
        if (event.getName().equals("ping")) {  
            event.reply("Pong!").queue();  
        }  
    }  
}
```
### 2. D4J (deprecated)  
```java
import sx.blah.discord.api.events.EventSubscriber;  
import sx.blah.discord.handle.impl.events.guild.GuildCommandEvent;  
public class PingBot {  
    @EventSubscriber  
    public void onCommand(GuildCommandEvent event) {  
        if (event.getCommand().equalsIgnoreCase("ping")) {  
            event.getChannel().sendMessage("Pong!");  
        }  
    }  
}
```
---
## C# (.NET)  
### 1. Discord.Net – *best*  
```csharp
using Discord.Commands;  
using Discord.WebSocket;  
var client = new DiscordSocketClient();  
var commands = new CommandService();  
client.Log += msg => Console.WriteLine(msg);  
client.MessageReceived += async msg =>  
{  
    var context = new SocketCommandContext(client, msg);  
    if (msg.Content.StartsWith("!"))  
        await commands.ExecuteAsync(context, 1, null);  
};  
commands.CreateCommand("ping").Do(async e => await e.Channel.SendMessageAsync("Pong!"));  
await client.LoginAsync(TokenType.Bot, "TOKEN");  
await client.StartAsync();  
```
### 2. DisCatSharp  
```csharp
var client = new DiscordClient(new DiscordConfiguration()  
{  
    Token = "TOKEN",  
    TokenType = TokenType.Bot,  
});  
client.MessageCreated += async (s, e) =>  
{  
    if (e.Message.Content == "!ping")  
        await e.Message.RespondAsync("Pong!");  
};  
await client.ConnectAsync();  
```
---
## Go (Golang)  
### 1. dgrijalva/discordgo – *best*  
```go
package main  
import (  
	"github.com/bwmarrin/discordgo"  
)  
func main() {  
	dg, _ := discordgo.New("BOT TOKEN")  
	dg.AddHandler(func(s *discordgo.Session, m *discordgo.MessageCreate) {  
		if m.Content == "!ping" {  
			s.ChannelMessageSend(m.ChannelID, "Pong!")  
		}  
	})  
	dg.Open()  
	select {}  
}  
```
### 2. samdoshi/gosora (inactive)  
Does not directly support slash commands, but can be done via HTTP.
---
## Rust  
### 1. serenity – *best*  
```rust
use serenity::prelude::*;  
use serenity::model::prelude::*;  
use serenity::framework::standard::{Command, CommandResult, macros::command};  
#[tokio::main]  
async fn main() {  
    let mut client = Client::new("TOKEN", MyHandler).await.expect("Error creating client");  
    client.start().await.unwrap();  
}  
struct MyHandler;  
#[serenity::async_trait]  
impl EventHandler for MyHandler {}  
#[command]  
async fn ping(ctx: &Context, msg: &Message) -> CommandResult {  
    msg.channel_id.say(&ctx.http, "Pong!").await?;  
    Ok(())  
}  
```
---
## Ruby  
### 1. discordrb – *best*  
```ruby
require 'discordrb'  
bot = Discordrb::Bot.new token: 'TOKEN'  
bot.slash_command(:ping, "Sends Pong!") do |event|  
  event.respond "Pong!"  
end  
bot.run  
```
---
## PHP  
### 1. HCGames/DisPHPatch – *best*  
```php
use DisPHPatch\DisPHPatch;  
$bot = new DisPHPatch('TOKEN');  
$bot->onCommand('ping', function ($interaction) {  
    $interaction->reply('Pong!');  
});  
$bot->run();  
```
---
## TypeScript  
### 1. discord.js (TS) – *best*  
```ts
import { Client, GatewayIntentBits } from 'discord.js';  
const client = new Client({ intents: [] });  
client.on('interactionCreate', async interaction => {  
    if (!interaction.isChatInputCommand()) return;  
    if (interaction.commandName === 'ping') {  
        await interaction.reply('Pong!');  
    }  
});  
client.login('TOKEN');  
```
---
## Dart / Flutter  
### 1. dart-discord – *best*  
```dart
import 'package:discord/discord.dart';  
void main() {  
  final discord = Discord(token: 'TOKEN');  
  discord.onMessage.listen((message) {  
    if (message.content == '!ping') {  
      message.reply('Pong!');  
    }  
  });  
}  
```
---
## Swift (iOS)  
### 1. SwiftDiscord – *best*  
```swift
import SwiftDiscord  
let bot = Discord(token: "TOKEN")  
bot.onMessage { message in  
    if message.content == "!ping" {  
        message.reply("Pong!")  
    }  
}  
bot.connect()  
```
---
## Kotlin (Android)  
### 1. JDA + Kotlin DSL – *best*  
```kotlin
val jda = JDABuilder.createDefault("TOKEN").build()  
jda.addEventListener(object : ListenerAdapter() {  
    override fun onSlashCommandInteraction(event: SlashCommandInteractionEvent) {  
        if (event.name == "ping") {  
            event.reply("Pong!").queue()  
        }  
    }  
})  
```
---
## C++  
### 1. D++ – *best*  
```cpp
#include <dpp/dpp.h>  
int main() {  
    dpp::cluster bot("TOKEN");  
    bot.on_slashcommand([](const dpp::slashcommand_t& event) {  
        if (event.command.get_name() == "ping") {  
            event.reply("Pong!");  
        }  
    });  
    bot.start(false);  
}  
```
---
## Lua  
### 1. discordia – *best*  
```lua
local discordia = require('discordia')  
local client = discordia.newClient()  
client:on('messageCreate', function(message)  
    if message.content == '!ping' then  
        message:reply('Pong!')  
    end  
end)  
client:run('TOKEN')  
```
---
## Perl  
### 1. Bot::Discord – *best*  
```perl
use Bot::Discord;  
my $bot = Bot::Discord->new(token => 'TOKEN');  
$bot->on_message(sub {  
    my ($self, $msg) = @_;  
    if ($msg->{content} eq '!ping') {  
        $self->send($msg->{channel_id}, 'Pong!');  
    }  
});  
$bot->run;  
```
---
## R  
### 1. rDiscord – *best*  
```r
library(rDiscord)  
bot <- discord_bot("TOKEN")  
bot$on_message(function(message) {  
    if (message$content == "!ping") {  
        message$reply("Pong!")  
    }  
})  
bot$start()  
```
---
## Elixir  
### 1. Nostrum – *best*  
```elixir
defmodule PingBot do  
  use Nostrum.Bot  
  def handle_message({:message_create, msg}) do  
    if msg.content == "!ping" do  
      Nostrum.Api.create_message(msg.channel_id, "Pong!")  
    end  
  end  
end  
PingBot.start("TOKEN")  
```
---
## Clojure  
### 1. clojure-discord – *best*  
```clojure
(ns pingbot.core  
  (:require [clojure-discord.core :as discord]))  
(discord/on-message (fn [msg]  
                      (when (= (:content msg) "!ping")  
                        (discord/reply msg "Pong!"))))  
(discord/start "TOKEN")  
```
---
## Haskell  
### 1. discord-haskell – *best*  
```haskell
import Discord  
import Discord.Types  
import Control.Monad.Trans (liftIO)  
main = do  
  runToken "TOKEN" $ do  
    on (\(MessageCreate m) -> do  
        when (messageText m == "!ping") $  
          liftIO $ sendMessage (messageChannel m) "Pong!")
```
---
## Scala  
### 1. Nyxx – *best*  
```scala
import io.github.nyx_discord.lib._  
val client = new Nyxx("TOKEN")  
client.on[SlashCommandInteractionEvent].foreach { event =>  
  if (event.command.name == "ping") {  
    event.reply("Pong!").complete()  
  }  
}  
```
---
## F#  
### 1. Discord.Net (F#) – *best*  
```fsharp
open Discord  
open Discord.Commands  
let client = new DiscordSocketClient()  
let cmd = new CommandService()  
client.MessageReceived.Add(fun msg ->  
    if msg.Content = "!ping" then  
        msg.Channel.SendMessageAsync("Pong!") |> ignore  
)  
client.LoginAsync(TokenType.Bot, "TOKEN") |> Async.AwaitTask |> Async.RunSynchronously  
client.StartAsync() |> Async.AwaitTask |> Async.RunSynchronously  
```
---
## Groovy  
### 1. Groovy + Java Discord API – *best*  
```groovy
import net.dv8tion.jda.api.*  
def jda = JDABuilder.createDefault("TOKEN").build()  
jda.addEventListener(new EventListener() {  
    void onEvent(Event event) {  
        if (event instanceof SlashCommandInteractionEvent && event.name == "ping") {  
            event.reply("Pong!").queue()  
        }  
    }  
})  
```
---
## Shell / Bash  
### 1. curl + Discord REST API – *best*  
```bash
curl -X POST \  
  -H "Authorization: Bot TOKEN" \  
  -H "Content-Type: application/json" \  
  -d '{"name":"ping","description":"Sends Pong!"}' \  
  https://discord.com/api/v10/applications/APP_ID/commands  
```
---
## VB.NET  
### 1. Discord.Net (VB) – *best*  
```vb
Dim client As New DiscordSocketClient()  
AddHandler client.MessageReceived, Sub(msg)  
                                      If msg.Content = "!ping" Then  
                                          msg.Channel.SendMessageAsync("Pong!")  
                                      End If  
                                  End Sub  
Await client.LoginAsync(TokenType.Bot, "TOKEN")  
Await client.StartAsync()  
```
---
## Objective-C  
### 1. ObjC-Discord – *best*  
```objc
#import <ObjC-Discord/Discord.h>  
DiscordClient *client = [[DiscordClient alloc] initWithToken:@"TOKEN"];  
[client onMessage:^(DiscordMessage *msg) {  
    if ([msg.content isEqualToString:@"!ping"]) {  
        [msg.channel sendMessage:@"Pong!"];  
    }  
}];  
[client connect];  
```
---
## Julia  
### 1. Discord.jl – *best*  
```julia
using Discord  
client = Client("TOKEN")  
on(client, :message_create) do msg  
    if msg.content == "!ping"  
        send(msg.channel_id, "Pong!")  
    end  
end  
run(client)  
```
---
## MATLAB  
### 1. webwrite + Discord API – *best*  
```matlab
url = 'https://discord.com/api/v10/applications/APP_ID/commands';  
headers = {'Authorization', 'Bot TOKEN'};  
webwrite(url, struct('name','ping','description','Sends Pong!'), 'HeaderFields', headers);  
```
---
## COBOL  
### 1. No ready-made libraries, but can be done via HTTP  
```cobol
IDENTIFICATION DIVISION.  
PROGRAM-ID. PING-BOT.  
ENVIRONMENT DIVISION.  
CONFIGURATION SECTION.  
REPOSITORY.  
    FUNCTION ALL INTRINSIC.  
DATA DIVISION.  
WORKING-STORAGE SECTION.  
01 URL PIC X(100) VALUE "https://discord.com/api/v10/...".  
PROCEDURE DIVISION.  
    DISPLAY "Pong!".  
END PROGRAM.  
```
---
## Fortran  
### 1. Use HTTP requests  
```fortran
program pingbot  
    print *, "Pong!"  
end program  
```
---
## Assembly (x86)  
### 1. Example of outputting text  
```asm
section .data  
    msg db 'Pong!', 0xa  
    len equ $ - msg  
section .text  
    global _start  
_start:  
    mov eax, 4  
    mov ebx, 1  
    mov ecx, msg  
    mov edx, len  
    int 0x80  
    mov eax, 1  
    xor ebx, ebx  
    int 0x80  
```
---
## 🎫 Research  
Below is a **comparative table of Discord API libraries in all programming languages**, including popular, obscure, and obsolete ones. The table includes:
- Language  
- Library name  
- Status (active/deprecated)  
- Support for Slash Commands  
- Ease of use  
- Performance  
- Documentation  
- Recommended for this language  

---
## 📊 Table: Discord API Libraries by Language  
| № | Language | Library Name | Active? | Slash Commands | Ease | Performance | Docs | Recommended |
|----|------|----------------------|-----------|----------------|------------|--------------|---------|----------------|
| 1 | JavaScript (Node.js) | discord.js | ✅ Yes | ✅ Yes | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ✅ Yes |
| 2 | JavaScript | eris | ✅ Yes | ✅ Yes | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ❌ |
| 3 | JavaScript | yui.js | ⚠️ Experimental | ✅ Yes | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ | ❌ |
| 4 | JavaScript | discord-harmony | ❌ Deprecated | ❌ | ⭐⭐ | — | ⭐ | ❌ |
| 5 | Python | discord.py | ✅ Yes | ✅ Yes | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ✅ Yes |
| 6 | Python | nextcord | ✅ Yes | ✅ Yes | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ | ❌ |
| 7 | Python | py-cord | ✅ Yes | ✅ Yes | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ | ❌ |
| 8 | Python | disnake | ✅ Yes | ✅ Yes | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ | ❌ |
| 9 | Java | JDA | ✅ Yes | ✅ Yes | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ | ✅ Yes |
| 10 | Java | D4J | ❌ Deprecated | ❌ | ⭐⭐ | — | ⭐ | ❌ |
| 11 | C# (.NET) | Discord.Net | ✅ Yes | ✅ Yes | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ | ✅ Yes |
| 12 | C# | DisCatSharp | ✅ Yes | ✅ Yes | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | ❌ |
| 13 | Go | discordgo | ✅ Yes | ✅ Yes | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | ✅ Yes |
| 14 | Go | gosora | ❌ Unsupported | ⚠️ Via HTTP | ⭐⭐ | ⭐ | ⭐ | ❌ |
| 15 | Rust | serenity | ✅ Yes | ✅ Yes | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | ✅ Yes |
| 16 | Ruby | discordrb | ✅ Yes | ✅ Yes | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ | ✅ Yes |
| 17 | PHP | DisPHPatch | ✅ Yes | ✅ Yes | ⭐⭐⭐ | ⭐⭐ | ⭐⭐ | ✅ Yes |
| 18 | TypeScript | discord.js (TS) | ✅ Yes | ✅ Yes | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ✅ Yes |
| 19 | Dart / Flutter | dart-discord | ✅ Yes | ✅ Yes | ⭐⭐⭐ | ⭐⭐ | ⭐⭐ | ✅ Yes |
| 20 | Swift (iOS) | SwiftDiscord | ✅ Yes | ✅ Yes | ⭐⭐⭐ | ⭐⭐ | ⭐⭐ | ✅ Yes |
| 21 | Kotlin (Android) | JDA (Kotlin) | ✅ Yes | ✅ Yes | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ | ✅ Yes |
| 22 | C++ | D++ | ✅ Yes | ✅ Yes | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | ✅ Yes |
| 23 | Lua | discordia | ✅ Yes | ✅ Yes | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ | ✅ Yes |
| 24 | Perl | Bot::Discord | ⚠️ Rarely updated | ⚠️ Via HTTP | ⭐⭐ | ⭐ | ⭐ | ✅ Yes |
| 25 | R | rDiscord | ⚠️ Limited | ⚠️ Via HTTP | ⭐⭐ | ⭐ | ⭐ | ✅ Yes |
| 26 | Elixir | Nostrum | ✅ Yes | ✅ Yes | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | ✅ Yes |
| 27 | Clojure | clojure-discord | ⚠️ Low activity | ⚠️ Via HTTP | ⭐⭐ | ⭐ | ⭐ | ✅ Yes |
| 28 | Haskell | discord-haskell | ⚠️ Rarely updated | ✅ Yes | ⭐⭐ | ⭐⭐ | ⭐ | ✅ Yes |
| 29 | Scala | Nyxx | ✅ Yes | ✅ Yes | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ | ✅ Yes |
| 30 | F# | Discord.Net (F#) | ✅ Yes | ✅ Yes | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ | ✅ Yes |
| 31 | Groovy | Discord.Net (Groovy) | ✅ Yes | ✅ Yes | ⭐⭐⭐ | ⭐⭐ | ⭐⭐ | ✅ Yes |
| 32 | Shell / Bash | curl + REST | ✅ Yes | ✅ Yes | ⭐⭐ | ⭐⭐ | — | ✅ Yes |
| 33 | VB.NET | Discord.Net (VB) | ✅ Yes | ✅ Yes | ⭐⭐⭐ | ⭐⭐ | ⭐⭐ | ✅ Yes |
| 34 | Objective-C | ObjC-Discord | ⚠️ Limited | ⚠️ Via HTTP | ⭐⭐ | ⭐ | ⭐ | ✅ Yes |
| 35 | Julia | Discord.jl | ✅ Yes | ✅ Yes | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ | ✅ Yes |
| 36 | MATLAB | webwrite + REST | ✅ Yes | ✅ Yes | ⭐⭐ | ⭐ | — | ✅ Yes |
| 37 | COBOL | — | ❌ No direct | ⚠️ Via HTTP | ⭐ | — | — | ✅ Yes |
| 38 | Fortran | — | ❌ No direct | ⚠️ Via HTTP | ⭐ | — | — | ✅ Yes |
| 39 | Assembly | — | ❌ Only low-level | ⚠️ Via HTTP | ⭐ | — | — | ✅ Yes |
---
## 🧭 Legend  
| Symbol | Meaning |
|--------|---------|
| ✅ | Fully supported |
| ⚠️ | Partially supported or via external tools |
| ❌ | Not directly supported |
| ⭐⭐⭐⭐⭐ | Very high level |
| ⭐⭐⭐⭐ | High level |
| ⭐⭐⭐ | Medium level |
| ⭐⭐ | Low level |
| ⭐ | Very low level |
| — | No data or not applicable |
---
## 📝 Notes  
- For some languages, there are no specialized libraries, but you can use the **Discord REST API** directly using `HTTP` requests.

---
## 🔍 Summary  
This table will help you choose the right library for working with the Discord API in any programming language.  
For most tasks, the following combinations are recommended:
| Language | Recommended Library |
|------|----------------------------|
| JavaScript | discord.js |
| Python | discord.py |
| Java | JDA |
| C# | Discord.Net |
| Go | discordgo |
| Rust | serenity |
| Ruby | discordrb |
| PHP | DisPHPatch |
| Kotlin | JDA |
| Swift | SwiftDiscord |
| C++ | D++ |
| Lua | discordia |
| Dart | dart-discord |
| TypeScript | discord.js |
| Shell/Bash | REST API |
| Elixir | Nostrum |
| Haskell | discord-haskell |
| Julia | Discord.jl |
| Objective-C | ObjC-Discord |
| F#, Groovy, VB.NET | Discord.Net |
| MATLAB, Fortran, COBOL, Assembly | REST API (manually) |

📌 **Tip:** If you're a beginner developer, choose a language you're familiar with and use the corresponding best library from this table.

---
## Conclusion  
This project shows that **there is no perfect language**, but there are convenient tools for each one.  
The rating is based on the author's opinion and popularity of the library — no one forces you to use anything!

Good luck)

If you find any errors, please let me know! I would also love to see new ways to launch Discord bots!

## Discord: pavlyska  
## Telegram: @pavlyska  

---

✅ **Note**: This version is an English translation of the original Russian document.
