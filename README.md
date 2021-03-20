# Grid_Terminal

Way Back the Fast libs (author MikhailV) enabled Blitz#d users to do awesome things for the time.

My handle on the Blitz forums was _33:

https://mojolabs.nz/posts.php?topic=69541

https://mojolabs.nz/posts.php?topic=77810

This was my work log for what I called "Grid World" but in fact it has mostly just Grid Terminal" log information as I have put aside a lot of the main Grid World work.

https://mojolabs.nz/userlog.php?user=10319&log=1460

////PASTE OF THE LOG HERE////
Grid World not dead but on standby	2008-04-11

I've worked on a couple other projects meanwhile, I did try some stuff on Grid World but it hasn't really moved. It is still a game I wish to make and I feel it is possible for me to do it. I tried some technologies and wasn't able to do anything good, specially with the shadow system. I also wish I could use a global lighing system like SSAO to help out.

I haven't been able to make models that are compatible to any shadow systems. I am still not good enough generating models that are closed. Maybe someday this will change, which would enable me to use some shadow systems that are available to me now.


Lines scheme	2007-11-05

I'm replacing all the underline and strikeout stuff and adding instead a whole LINES scheme in the terminal system. I'm allowing 1 byte per character specifically for lines drawing capabilities. What this enables, is that there will be possibility of a line on ;top, bottom (underline), left, right, horizontal centered (strikeout), vertical centered, 45° cross angle upwards, 45° cross angle downwards.

This should permit me later to add boxes around characters or key entry fields, and do all sorts of other outworldly tricks :P

So, as a souvernir, here is the "old" strikeout and underline drawing code set for deletion:
format_codebox(' ; strikeout
argb% = PeekInt (palette_table, term\page[page_id]\strikeout_color Shl 2)
SetColor (argb Shr 16) And $ff, (argb Shr 8) And $ff, argb And $ff
Local strikeout_len% = 0
Local strikeout_thickness% = Floor(char_height * 0.04) + 1
SetLineWidth strikeout_thickness
SetBlend FI_ALPHABLEND
SetAlpha term\page[page_id]\text_alpha
If special_features And PGDPS_SF_UPDATE_CHARS_ON Then
For row = start_row To end_row
For col = start_col To end_col
disp = row * cols + col
If PeekByte(item_table, disp) = mouse_saved_item Then
strikeout = (PeekByte(attr_mask_table, disp) And TXRNA_STRIKEOUT_ON)
Else
strikeout = (PeekByte(attr_table, disp) And TXRNA_STRIKEOUT_ON)
EndIf
If strikeout Then
If (display_state And PGDPS_SF_UPDATE_ANGLE_ON) > False Then
SetRotation PeekByte(angle_table, disp) * 2
EndIf
If (display_state And PGDPS_SF_UPDATE_ALPHA_ON) > False Then
SetAlpha PeekByte(alpha_table, disp) * 0.003921569
EndIf
If (display_state And PGDPS_SF_UPDATE_ZOOM_ON) > False Then
SetScale PeekByte(x_zoom_table, disp) * 0.015625, PeekByte(y_zoom_table, disp) * 0.015625
EndIf
If (display_state And PGDPS_SF_UPDATE_OFFSET_ON) > False Then
SetOrigin PeekByte(x_offset_table, disp) - 128, PeekByte(y_offset_table, disp) - 128
EndIf
SetColor (argb Shr 16) And $ff, (argb Shr 8) And $ff, argb And $ff
DrawLineSimple col * char_width, (row + 1) * char_height - char_height * 0.5, (col + 1) * char_width, (row + 1) * char_height - char_height * 0.5
EndIf
Next
Next
Else
For row = start_row To end_row
For col = start_col To end_col
disp = row * cols + col
If PeekByte(item_table, disp) = mouse_saved_item Then
strikeout = (PeekByte(attr_mask_table, disp) And TXRNA_STRIKEOUT_ON)
Else
strikeout = (PeekByte(attr_table, disp) And TXRNA_STRIKEOUT_ON)
EndIf
If strikeout Then
If strikeout_len = 0 Then draw_start_col = col
strikeout_len = strikeout_len + 1
Else
If strikeout_len > 0 Then
draw_start_X = draw_start_col * char_width
draw_start_Y = row * char_height + Float char_height * 0.5
;DrawRectSimple draw_start_X, draw_start_Y, char_width * strikeout_len, strikeout_thickness, 1
DrawLineSimple draw_start_X, draw_start_Y, draw_start_X + char_width * strikeout_len, draw_start_Y
strikeout_len = 0
EndIf
EndIf
Next
If strikeout_len > 0 Then
draw_start_X = draw_start_col * char_width
draw_start_Y = row * char_height + Float char_height * 0.5
;DrawRectSimple draw_start_X, draw_start_Y, char_width * strikeout_len, strikeout_thickness, 1
DrawLineSimple draw_start_X, draw_start_Y, draw_start_X + char_width * strikeout_len, draw_start_Y

strikeout_len = 0
EndIf
Next
EndIf

; underline
Local underline_len% = 0
Local underline_tickness% = char_height * 0.04 + 1
SetLineWidth underline_tickness
argb% = PeekInt (palette_table, term\page[page_id]\underline_color Shl 2)
SetColor (argb Shr 16) And $ff, (argb Shr 8) And $ff, argb And $ff
SetBlend FI_ALPHABLEND
SetAlpha term\page[page_id]\text_alpha
If special_features And PGDPS_SF_UPDATE_CHARS_ON Then
For row = start_row To end_row
For col = start_col To end_col
disp = row * cols + col
If PeekByte(item_table, disp) = mouse_saved_item Then
underline% = (PeekByte(attr_mask_table, disp) And TXRNA_UNDERLINE_ON)
Else
underline% = (PeekByte(attr_table, disp) And TXRNA_UNDERLINE_ON)
EndIf
If underline Then
If (display_state And PGDPS_SF_UPDATE_ANGLE_ON) > False Then
SetRotation PeekByte(angle_table, disp) * 2
EndIf
If (display_state And PGDPS_SF_UPDATE_ALPHA_ON) > False Then
SetAlpha PeekByte(alpha_table, disp) * 0.003921569
EndIf
If (display_state And PGDPS_SF_UPDATE_ZOOM_ON) > False Then
SetScale PeekByte(x_zoom_table, disp) * 0.015625, PeekByte(y_zoom_table, disp) * 0.015625
EndIf
If (display_state And PGDPS_SF_UPDATE_OFFSET_ON) > False Then
SetOrigin PeekByte(x_offset_table, disp) - 128, PeekByte(y_offset_table, disp) - 128
EndIf
SetColor (argb Shr 16) And $ff, (argb Shr 8) And $ff, argb And $ff
DrawLineSimple col * char_width, (row + 1) * char_height - 1, (col + 1) * char_width, (row + 1) * char_height - 1
EndIf
Next
Next
Else
For row = start_row To end_row
For col = start_col To end_col
disp = row * cols + col
If PeekByte(item_table, disp) = mouse_saved_item Then
underline% = (PeekByte(attr_mask_table, disp) And TXRNA_UNDERLINE_ON)
Else
underline% = (PeekByte(attr_table, disp) And TXRNA_UNDERLINE_ON)
EndIf
If underline Then
If underline_len = 0 Then draw_start_col = col
underline_len = underline_len + 1
Else
If underline_len > 0 Then
draw_start_X = draw_start_col * char_width
draw_start_Y = row * char_height + char_height - 1
DrawLineSimple draw_start_X, draw_start_Y, draw_start_X + char_width * underline_len, draw_start_Y
underline_len = 0
EndIf
EndIf
Next
If underline_len > 0 Then
draw_start_X = draw_start_col * char_width
draw_start_Y = row * char_height + char_height - 1
DrawLineSimple draw_start_X, draw_start_Y, draw_start_X + char_width * underline_len, draw_start_Y
underline_len = 0
EndIf
Next
EndIf

EndDraw
EndIf')


ramblings about slowness	2007-10-27

I bought the PhysX module :)

Terminal:
- I've come to fix a lot of the issues in scaling 2D on to various physical screen tracing resolution. Basically, any screen resolution will work and display the same result now, from a virtual screen stand point. Simply put, I've implemented a virtual resolution in the functionality of the terminal, which permits to properly scale that to the actual screen resolution. It was quite simple to implement, finally, and it works pretty well. (Still some qwirks in some instances relating to character fonts being scaled which some time don't efficiently scale in some rare occasions, but that seems to be a matter of precision work which I'll resolve later on. *FIXED*)
- Finally found a classy way of defining pages that are portion of a fullscreen in size. What I use is a FLOAT for width and height, and take in number of character per column and per row. Compute that with the virtual resolution to get the final width and height in virtual pixels and we're all set!
- Continued work on the shadowing, but need intense testing on those. I've finally added character shadow, so that's a nice addition, but quite frankly, it doubles the load on the graphics and in my opinion isn't really efficient for what it adds. plotting shadow pixels direct in the font is much more interesting for performance.
- Figured a way to properly do a resolution change. Implemented clear/init fonts functions and added extra padding to make sure nothing goes wrong. First thing to do, is perform a call to set the user resolution values, then you do a call to clear the fonts out of the memory, and finally you launch a term_init_display() through script and finally initialize the internal fonts. Lastly, to complete the task, if there were any loaded fonts, well they are lost. So a load of the user fonts will do. If there were 3D models and other graphics things loaded, they are lost, so, it should be considered down the line. But, it works ;)
- Adding LOAD and UNLOAD commands, which permits to load data in pages, palettes, fonts, and a bunch of other places, as well as getting data from all those places. So this goes in the term_receive_byte() and term_send_byte() pipeline and should get to be quite useful for transferring chunks of data around without much of a hassle about how the terminal works internally from an application standpoint. Not to mention that, this is as flexible as can be when dealing with networking and multiple instances of the terminal and shuffling low level data around.
- Adding encryption scheme. The point of this is that when the terminal system will be used for netplay and stuff, all the conversations and some data transfer may pass through an encode/decode process for a simple security hack. It's not meant to be the biggest most intense security scheme, but enough to keep some casual network game hackers out.

Game Engine:
- Well, I've tried a nice sky on the 3D scene, but then realized all the lighting didn't make any sense, so I hardcoded lighting values to see how things need to get changed in order for this to look like something, and voilà. More or less, it's getting used to lighting and parms to have things look normal.
- Worked on the object builder system to change the system from a 16 axis to a 24 axis type. Now the code looks a lot different than what it used to. But, the nice thing that this enhancement adds is the possibility of making slanted corners, which wasn't possible before. But still need to work on this a lot to ghave all the possibilities properly added and tested also. It's really a question of not breaking what is there and at the same time widening the spectrum of shapes that it can perform, with the proper culling, and so on and so forth. Some things that I am planning afer the 24 axis is to solder faces. That might be a performance enhancing feature, but might not fix the stencil shadowing problem that I have with most stencil shadow engines. The obvious goal would be to generate an optimized stencil shadow mesh.


term parser enhancements	2007-09-11
I've taken a break from my secret project and went back to working on my term system (Grid Terminal). I've set some new goals, as I wish to mature the terminal toolset some more and add more scripting functionality into the parser. Basically, I need to turn the ANSI/VT parser into a full scripting language :) Since the VT terminal standard is basically a parser for the display, I can improve it endlessly. But, the goal is to turn it into something more useful and broaden it's horizons.

The cool thing about writing ansi art or VT command stacking, is you can virtually design the whole screen and do all sort of funky stuff. Yet, the standard only contains some basic screen functions and some parms, nothing more. So I'm thinking of adding some possibilities such as some crude assembly like commands, such as CMP, BNE, and so on... In essence, I want it to be able to loop commands and do all sorts of funky stuff, so I don't have to code it in blitz3d itself, but just stack a bunch of commands that would permit doing logical operations, in a VT fashion. So, the commands would of course be bytecode, just like what we're used to in ANSI. We'd get to do a BR to a label or what not.

Really what this means is I would have to develop a sort of START and END for the programs, or routines, and ways to stack the script in a buffer. When the script is finished, it will erase itself, if it is told to do so. There will be run once scripts, and scripts that will be stacked as a modifier. Hmmm, but I might change the meaning of modifiers, I'm not yet sure this is how I should do things. They could just be script buffers, but calling that modifiers, I'm not yet sure.

Anyway, this is not something I'll be able to complete in 2 days, because I also need to change the bytecodes and reshape the whole parser to accept all sorts of new syntax and logic.

I'll come back to post some news about this someday.

---------------------------

Since I haven't actually touched the game in over 4 months, I suppose it must be because I haven't bought the PhysX module yet. But I'm really slow and taking my time. But I do have a full game design to implement, and it should be quite interesting when I get to the actual game and have some AI action going on! But I like working on the interface right now with the term, that is evolving slowly.

I'm also used to code for bosses and divisions in companies, so not having any superiors waiting for anything from me is not helping the situation. And most of what I'm doing in Blitz3D ATM is really all newq stuff to me and I'm thinking about how to do things in the most professionnal manner possible. Often, I start coding portions, and later they get redesigned, variables renamed. Analysing code and it's meaning is a big portion of how I work. I perfect the stuff until I can actually expand it some more. Another trick I use is define my working space (or Type templates) first, and carefully think about the usage of every variable that I declare, so it makes as much sense as it should for the task that should be accomplished.


side project enhances term	2007-08-15
Well, aside from fixing a couple display bugs that was left there for over a month, I've added the possibility to sync the terminal to an outside source, thus CSI_set_timer. Now every page data, counters, blinking and whatnot can now be synced and reset as needed. This enables a non realtime application like a renderer to sync the terminal to it, which is needed for what I'm working on.

01 Sept. 07:
Adding 3D text entity functions to the term. It breaks a little with my tradition of having this 2D only and having the user to pass thru the bytecode parser. But as it stands, I've created a bunch of text-to-3D entity functions, which allows for some neat things. I have some plans for this with effects, but it's rather complex for me and I'm doing this a step at a time, so I can properly plan this all out. My hopes are to have fully customizable 3D text functions to be used as entities in a 3D environment. The text, at first is based on various primitives of choice and uses either the base fonts or fonts that are loaded from 1024x1024 / 512x512 / 256x256 pixel 32 bit font sheets. There are no bitmaps involved that I plan, but instead vertex colors. I may add ways to put text as a texture, but that's not the immediate goal.


Side Project	2007-08-15


This live screenshot is from a "secret" side project of mine, for a friend. I'm using of coruse Grid Terminal for this very classic color bar screen. Here is the function that draws this screen, all using built in fonts. It is called only once, and the terminal simply updates it, with the blinking, the cursor animation and what have you. The test layer over the color bar is a 64x48 text layer for my timecode. It's using another built in font.

Here is the source code that builds the color bar backdrop with the titling:format_code('Function color_bar(width% = 40, height% = 30, title$, artist$)

y_offset% = 0

c$ = ""

d$ = ""
d$ = d$ + Chr$(27) + "[160" + Chr$(CSI_set_bkgnd_color) + " "
d$ = d$ + Chr$(27) + "[161" + Chr$(CSI_set_bkgnd_color) + " "
d$ = d$ + Chr$(27) + "[162" + Chr$(CSI_set_bkgnd_color) + " "
d$ = d$ + Chr$(27) + "[163" + Chr$(CSI_set_bkgnd_color) + " "
d$ = d$ + Chr$(27) + "[164" + Chr$(CSI_set_bkgnd_color) + " "
d$ = d$ + Chr$(27) + "[165" + Chr$(CSI_set_bkgnd_color) + " "
d$ = d$ + Chr$(27) + "[166" + Chr$(CSI_set_bkgnd_color) + " "
For y = 0 To 20
c$ = c$ + Chr$(27) + "[0;" + Str$(y_offset + y) + Chr$(CSI_cursor_update) + d$
Next
d$ = ""
d$ = d$ + Chr$(27) + "[166"+Chr$(CSI_set_bkgnd_color)+" "
d$ = d$ + Chr$(27) + "[169"+Chr$(CSI_set_bkgnd_color)+" "
d$ = d$ + Chr$(27) + "[164"+Chr$(CSI_set_bkgnd_color)+" "
d$ = d$ + Chr$(27) + "[169"+Chr$(CSI_set_bkgnd_color)+" "
d$ = d$ + Chr$(27) + "[162"+Chr$(CSI_set_bkgnd_color)+" "
d$ = d$ + Chr$(27) + "[169"+Chr$(CSI_set_bkgnd_color)+" "
d$ = d$ + Chr$(27) + "[160"+Chr$(CSI_set_bkgnd_color)+" "
For y = 21 To 22
c$ = c$ + Chr$(27) + "[0;" + Str$(y_offset + y) + Chr$(CSI_cursor_update) + d$
Next
d$ = ""
d$ = d$ + Chr$(27) + "[167"+Chr$(CSI_set_bkgnd_color)+" "
d$ = d$ + Chr$(27) + "[31"+Chr$(CSI_set_bkgnd_color)+" "
d$ = d$ + Chr$(27) + "[168"+Chr$(CSI_set_bkgnd_color)+" "
d$ = d$ + Chr$(27) + "[169"+Chr$(CSI_set_bkgnd_color)+" "
d$ = d$ + Chr$(27) + "[0"+Chr$(CSI_set_bkgnd_color)+" "
d$ = d$ + Chr$(27) + "[169"+Chr$(CSI_set_bkgnd_color)+" "
d$ = d$ + Chr$(27) + "[170"+Chr$(CSI_set_bkgnd_color)+" "
d$ = d$ + Chr$(27) + "[0"+Chr$(CSI_set_bkgnd_color)+" "
For y = 23 To 29
c$ = c$ + Chr$(27) + "[0;" + Str$(y_offset + y) + Chr$(CSI_cursor_update) + d$
Next
c$ = c$ + Chr$(27) + "[0;29" + Chr$(CSI_cursor_update)

term_send_command(CSI_cursor_home)
term_send_command(CSI_set_graphic_rendition, 10, 192, 143, 253)
term_send_string(c$)

term_send_command(CSI_set_graphic_rendition, 1, 3, 7, 10)
term_send_command(CSI_cursor_update, 20 - Len(title$),10) : term_send_command(CSI_set_mode, 140) : term_send_string(title$) : term_send_command(CSI_reset_mode, 140)
term_send_command(CSI_cursor_update, 18,12) : term_send_command(CSI_set_mode, 140) : term_send_string("by") : term_send_command(CSI_reset_mode, 140)
term_send_command(CSI_cursor_update, 20 - Len(artist$),14) : term_send_command(CSI_set_mode, 140) : term_send_string(artist$) : term_send_command(CSI_reset_mode, 140)

term_send_command(CSI_set_graphic_rendition, 6)
term_send_command(CSI_cursor_update, 9,18) : term_send_command(CSI_set_mode, 141, 143) : term_send_string("STANDBY") : term_send_command(CSI_reset_mode, 141, 143)
term_send_command(CSI_cursor_update, 9,19) : term_send_command(CSI_set_mode, 141, 143) : term_send_string("STANDBY") : term_send_command(CSI_reset_mode, 141, 143)
term_send_command(CSI_set_graphic_rendition, 0, 3, 14, 132)
term_send_command(CSI_set_style, 11, $08)
term_send_command(CSI_cursor_update, 38,20)
term_send_command(CSI_reset_mode, 140, 143)
term_send_string("33")
term_send_command(CSI_set_graphic_rendition, 0, 3, 14, 140)
term_send_command(CSI_cursor_update, 38,19)
term_send_string(" ")
term_send_command(CSI_cursor_update, 37,20)
End Function
')

Of course, while I'm working on this project, i'm also finding some old bugs I'm sqwashing at the same time, and improving some stuff that I need as I go along.


Fewer updates	2007-08-11

There will be fewer updates from now on, as I have another project to worry about, which is very urgent! But the Terminal emulator engine will still get some upgrades, but rare info.


terminal evolves	2007-08-05

Here's a recap of what's done and what's not done:
- Complete the page shadow system (chars, cursors) *** 50% DONE, INCOMPLETE
Still needs to have the background and a bunch of other stuff completed for this, but the basic is there in the page render and most should already work, but not yet tested.

- Standardize the display update in regards to the page display order list. *** 25% DONE, INCOMPLETE
It's very complex to have this sorted out and generalize the logic of this. It's not fully tought out.

- Complete the special features segment (background, shadow) *** 100% DONE, COMPLETED
That wasn't as hard as I tought! Every segment of the page rendering has the needed stuff for this to work, and it does, so I'm happy.

- create CSI_process_layer_zone (this will be a _HUGE_ help for special effects) *** 20% DONE, INCOMPLETE
That's the most complex thing I've ever come across so far for the terminal development. It's not yet sorted out as to how I'm going to make this, so there is no fast solution for completing this. (this is what I call "modifiers")

- create CSI_set_page_size (this is like a global zoom of a page, I don't know yet how good it will look, but I am very sceptic on the results) *** COMPLETED

- Optimize tokenizer with using a bank instead of a token type *** COMPLETED

- re-organize some of the CSIs so there are less misleads and confusion in the way they should be used *** COMPLETED

- Complete manual (5% done so far :P) *** 5% DONE, STILL

Next, I've added in the special features segment, the shades section, where it is possible now to shade each character individually from one color to the next, or to shade from one alpha value to the next. It's a big visual enhancement for anyone who uses this.

I'm playing with some easy fonts also (8 x 8 pixels in one bit plane). At least the terminal is now equiped with 4 different fonts. I need to add some options in the font builder thoe, where it would be possible to have font contours, and shadow drop capability while rendering the font. I've figured out a way to add this, so it's definately on my to-do list.

I'm thinking of a way to add gamescript to the terminal master mode, so it could launch a script, just as if it was booting from a rom image :P. This is in the very early stages. But I've tought of the capabilities from the script point of view and how this would all be assembled. I'm still worried on how I'll send parms to the script from the terminal. It could be from the interlnal registers of the terminal, not yet decided on this.

August 6th:
Character shading has been still worked on, and now shading is possible in 4 directions on color and alpha. The way it works on color is it picks up the color of the character in the direction the shading is looking at. So if I select to shade upwards, it will take the next color up, and it wraps (meaning if I'm on the top line, it will pick the color from the bottom). The same logic is applied for character alpha values, which have alpha shading possible on 4 directions + picking the next color. I might improve this, but for now it does what I intended to do.

The performance hit taken by the SF segment is minimal when not used, and around 10% when everything is used except rotation and scaling. Rotation takes a lot in framerate as there is a play in zbuffer going on. The same can be said with scaling. Both of these can't be improved upon, unless Blitz3D get's a new update fixing the zbuffer performance issue.

August 8th:
Everything that has to do with keyboard handling has been revisited and expanded, clarified.


Term: modifiers & registers	2007-07-29
With a maturing 2D terminal in a 3D world, there has to come some serious capabilities. And so, I've tought of the following... Bare with me these are experimental to me so I might be little clumsy in my explanation. Also, these are their first iteration, and might change in the process. So, the goal is to have special effects. Ah, yes... We can all dream of those scroll texts, parallax, plasma, and what not effects. It's the golden age of the demo scene as we all know it. And so, I had some deep toughts about a neat way to implement the capabilities into this terminal, so it would be easy to access and trigger, as well as very flexible. Part of me wanted something general purpose, the other par tof me says "do me this effect, or that type of math function", and so on... Having an overlook at all the possible combination of effects that I wish to implement, I decided to generalize the scheme into two very singular notions.

* The first notion is the notion of registers. We most likely all know what registers are used for in the computer world. Anyone that programmed assembler know what registers are. Anyone that touched MMX, SSE knows what registers are. here, it's the same concept. They are general purpose registers. I've tought of having 16 registers (for now) in 32 bit wide. What the terminal system will do, is just maintain the registers troughout the life of the terminal. And so, the registers will be an easy way to store and shuffle data.

* Next are the modifiers. Modifiers are type occurences that give the terminal the possibility to touch everything and anything in pages and to modify them, in an automated fashion. There are 4800 modifiers in the terminal (40 * 30 char pages times 4, for maximum flexibility). So, basically, you can have one modifier per character, and the modifier will treat the character layers as it pleases.

Here's the data structure of a single modifier:
mode% ; that's where we define modifier states (contains page number, on, off state, cycle once...)
UL_zone% ; upper left corner of page zone
DR_zone% ; down right corner of page zone
counter% ; well, that's the modifier counter
inc% ; that is the increment value of the modifier
dest% ; this is the destination list of the modifier
func% ; here's the function at hand

Now this is not final in any way. It even seems bogus at the moment from my perspective, but it is a starting point. The general idea will stay. It's very possible that modifiers will contain full scripts in byte code. So, it would be possible to have multiple functions performed.

How all this gets integrated in the terminal? Easy, you have modifiers, and you have registers. The modifiers use registers to do what they have to do. Every call to Update_Terminal() will effectively generate a call to Update_Modifiers(). In Update_Modifiers(), there will be a master loop across all modifiers that are active (because you could have a dead mofifier, or one that requires an update). The modifier get's updated, math gets processed, using registers, and then, what? Well, the modifier will go through it's assigned zone, and plot it's result as it goes along. Since it's a modifier, well, it's modifying the content of that zone by doing XOR, OR, AND, INC, DEC operations on the page layers at hand.

Like I said the data structure and the content of the modifier is not final, and I intend to revise this several times until I have a somewhat perfect system to perofrm ALL of what I envision for it. And when it's done, well, it's gonna be party time.

I'm trying to promise to myself that it'll be beer time then :P

Note: In regards to performance, I'm not too worried, as I can hardly imagine needing to use 4800 modifiers in the first place. And second, if I can display a full page with all bells and whistles at 170 FPS right now, I don't think modifiers will eat up enough CPU to bring that down to 60 FPS. I'm thinking more like 140 - 150 FPS for updating 4800 complex modifiers (so, 20 - 30 FPS from 170, that's like less than 20% of cpu time in the most severe case. But, when you'll have 4800 modifiers working for you, you'll also have a kick a** effect being processed at the same time.

EDIT: Further analysis.............. little discouraged with types ATM. Just querying the type 4800 times every frame dropped the framerate from 760 to ~700 fps in a relatively empty page. Dropped the framerate from 156 to ~151 fps on a real busy page. So, no action posed, just a query... Well here's a query:
format_code('Function Update_modifiers()
For mod_id% = 0 To (TRM_max_modifiers - 1)
If term\modifier[mod_id]\mode > 0 Then
EndIf
Next
End Function
')
Soooooo, what I'll probably do is simplify the memory structure and use a bank (again...) and it should bring all this together. Types can be little slow. But reading a bank usually yelds good performances, even thoe peek and poke SHOULD BE much faster than how it is currently in Blitz3D. Yet, we have to understand that we're dealing with protected memory and some virtualization. So in that regard, it is justified.

Jully 30th:

I'm a little pissed about the performance actually! I'll probably have to dig some more performance in this terminal system. My next performance move is to regroup all the layers of a page into a single bank. That should help! I'm positive that I'll save a nice chunk of processing time like this in all page data manipulation scenarios. Now remember, this term project is an accessory to a game, so it shouldn't take 90% of the CPU, nor 50%, nor 25%!!! 10% to 20% of total CPU usage for just the 2D menu should be enough of an overhead. That is basically how it performs right now, if we're going at 60 FPS and we've got a busy page environment. It can go up to 100% in 60 FPS if you have huge pages of 80x60 chars all fully busy in 4 page split mode. But that is the extreme case, not the realistic case.

-*-*-*-*-*-*-*-
I've done some tweaking in Render_page(). I've basically gave it about 5% to 8% (depending on what's displayed) speed increase. It's not really a lot. How I did it was simple; all I had to do is remove various function calls like getRGB() and term_get_palette_color(), then put the code directly in Render_page(), plus, remove the access to the type to get the handles, but instead get the handle only once, at the top of the function into an int.

It's pretty interesting how chaging some variables from float to int or back to float can alter performance. From what I experimented in the last few months, it would be preferred, in a performance point of view, to avoid shuffling data between floats and ints the least possible. When you stick to ints, you generally get the best performance, whatever the scenario.


More terminal goodies	2007-07-21
Never ending is the terminal sub project (near 3 months in the making). What I'm concentrating on right now is adding a "special features" segment. I've added some comments about this yesterday, but it's going amazingly well. So right now, the text layer has all the special features activated and working. So, now there are 21 layers for each terminal page that are all addressable and functionning. For the memory savy in me, that makes for example a 40 by 30 character screen around 25200 bytes, or roughly 25K bytes in size.

What this means is that it is now possible to address the following, on a per character basis:
- Alpha transparency (from nothing to full bright)
- X and Y Zoom (from nothing to 4x size)
- Angle (0° to 360° rotations)
- X and Y offsets (-128 to 127 pixels)
(and the older stuff)
- backrgound color (palette color, 0 to 255)
- character color (palette color, 0 to 255)
- character attributes (blink, bold, underline, strikeout...)
- 2 masks (mouse over mask, and mouse click mask)
I must repeat these are PER CHARACTER values, meaning every character has their own unique values for each of those parameters!

Adding the special features resulted in almost NO performance overhead (less than 1%). So it is definately interesting to play with and have in there. Now what I need to do is to program some effects ;), but that will be a little more involving, and might require some thinking. If I do add effects in the terminal system, that might lead into thinking that the only effects possible are the one that are pre-programmed within the system, which is not what I wish to do. I want a totally addressable and programmable terminal. Even so, that I need to speed up yet again the tokenizer as it's using types to store tokens, which is something I wish to change, for instance into using a generic token bank. This should increase the speed of the tokenizer 2 to 3 folds. Meaning, there won't be any excuses for not using tmy parser, as it will be pretty damn fast, and convenient.

I might add more special features, but for now, I'll have to contentrate of having this 100% completed on all fronts, and easily accessible.

July 25th:

I took a break last few days, wrote down some more specs, fixed minor issues with the special features segment.
I'm re-evaluating what is left to do for the term module:
- Complete the page shadow system (chars, cursors)
- Standardize the display update in regards to the page display order list.
- Complete the special features segment (background, shadow)
- create CSI_process_layer_zone (this will be a _HUGE_ help for special effects)
- create CSI_set_page_size (this is like a global zoom of a page, I don't know yet how good it will look, but I am very sceptic on the results)
- Optimize tokenizer with using a bank instead of a token type
- re-organize some of the CSIs so there are less misleads and confusion in the way they should be used
- Complete manual (5% done so far :P)

July 26th:

- Optimize tokenizer with using a bank instead of a token type *** DONE ***
This brings in a major speed increase! I had to adapt more or less 150 lines of code for this, and it's definately worth the little effort. It's quite a satisfaction when you run a test and you see the new way the tokens are generated and that it just runs flawlessly and several folds faster. I don't think it could get much faster, as I'm using PokeShort / PeekShort to store my tokens :P I don't need to initrialize or to blank out my tokens, as I rely solely on a tokencount ;) . It was a little task to handle the tokens, but still, running the called functions will still require some processor as most of what it's about is shuffling bank data. Now, the only true bottleneck is the graphics card speed. Some performance issues are still quite noticable when playing with alpha'd characters and background tiles, but it's quite possible it's a Blitz3D problem, and hopefully it will be addressed.
- create CSI_set_page_size *** DONE ***
Wasn't really involving to create. It works fine, but the tiles in the back are not flush depending on the size that is set. I put FLOATs to get the width and height size. 1.0 is the regular size. So 2.0 is double, 0.5 is half, 0.25 is 4 times smaller... So it's quite easy to make a page zoiom in and out! This is perfect for some title screen effects for example, combined with alpha values, I think it makes for a nice addition.

Julty 27th:

Major rewrite of all of the high level page data manipulation functions completed. These now take in effect the new special features segment. Created a CSI_set_style to set various writing attributes, including the special features. Now the special features is implemented full circle IMHO. Yet, the backgroudn tiles haven't been programmed for this yet, and I don't know if I should. Or maybe if I do, I'll put a mode switch for enabling/disabling special features for the background. I think that should cover the problem.


Still not satisfied with the Terminal	2007-07-07
Still not satisfied, but it's getting to be a nice little mature terminal system none the less. I could create a small demo which would probably be neat because of all the features that ended up in there since it all started (back around early may). But I wish I could move to expanding the Console and it's parser. Make the needed menu managment functions, and other, more in depth stuff. Move to actually building a game! Learn PhysX in Blitz3D... To actually have a good working demo with actual working realtime stencil shadows, for example.

But, there are still some things that need to be addressed before I can move on to the more meaty stuff. Most of those, sadly, are getting in the complexity zone. I'll try to enumerate the current sizeable issues:
- I'm still juggling with the code for page manipulation. I've removed a lot of hardcoded stuff. I've added a draw_list with levels (0 to 31) which contain page numbers, and various states. All this works rather well, until we get to the more obscure page handling. Still some hard coded parts for the page splitting stuff (which is a real bugger). It's very very hard to replicate manipulating page split without redesigning the whole system from scratch, or do like me and have a parallel system and mutate the old system to the new system, until you get to destroy the old code (rem it out, and bye bye...). This is very hard to bypass at the moment, and is something that need sto get addressed quickly, before I loose it!
- Only when the page + draw_list can work together in full circle, then it is possible to actually implement the page shadows. I've done some code fot that, but still haven't come to tyesting it as it is a little advanced into the project for now. It's code just waiting to be used. But basically, everything can be shadowed in the terminal (cursor, mouse, all windows, all tiles and text). Eventually it will all be there and working, and that's where I want to be NOW, but is too far ahead to actually imagine a result, but very feasible in the current implementation of the terminal.
- After these two big chunks done, I'm planning to expand the terminal concept into a full swing text demo platform, where you will be abel to do scoll text with funky effects, or for example, text that explode, or text that spin madly to then form phrases. This is still an iffy proposition, but I really am thinking of this, and quite often too.

July 10th:

Well, got some default internal fonts now (only 2). If anyone reading this can point me to some very very old system fonts, pixelated ones, that would be good! All 256 characters, it could be any sort of pixel font, 8x8 up to 64x64.

July 13th:

I've implemented a way for the terminal to perform self tests. Seems to work. But I have some problems to fix, but it's pretty neat to see a self test running and then to have the console appear.

After some tests and more tweaking of the self test mode, I've come to the conclusion that the terminal will need a setup mode also. Since 2 days ago, I've added Graphics3D management in the terminal, so the app that will use this won't have to worry about Graphics3D and setting up resolutions and some parameters. But it's not fully implemented. I will have to make a setup for this terminal and some menus and choosing the options. I'll have to make a terminal.cfg type file, that will be the place where the graphics parms will be stored. I'm still thinking on how I'm gonna do this, as the terminal doesn't have any structure for menus, but only clickable items. So, I'll probably just let the mouse choose the parameters, or I can make a function that reads the answerback buffer, as if it's a console app. That shouldn't be too hard actually. But it looks like long days ahead to implement all this.

July 14th:

- Sorted some stuff out for the new graphics3D management, but still no menu. So, that's a a couple of new functions, a new table, and new mode_id's, some various other bits of logic to make this minimalistically implemented. The concept should get expanded.
- I've started coding term_setup(), but it's going to be one big menu function, the first one in fact. So it's a pilot project for the menus. It's obvious that this will spawn a dozen more new functions as it gets mature. So it will have to take some time to code, before it will actually work.
- One thing that got finally added: term\mode... Now, most terminal states are centralized in a mode bit array, and implementation is complete. This shoul dhave been done way back, but I'm quite slow with this project.

July 19th:

- Small fixes to the way the terminal gets initialized and shut down.
- Small fixes to setting up the resolution, more precise checks, but nothing more
- More standardization of the term\mode with more modes hooked up in there
- 2 new commands: CSI_fill_layer_zone and CSI_write_big_character. Both commands enables writing values in block to a chosen layer. One is simply a block (zone) fill, the other is more subtile as it fills a block with character pixels from a chosen "in terminal memory" pixel font. Both are very useful IMHO.
- Created a user config file system, and added all the nessesary calls to the sts (set terminal state) command.
- Added a page clipboard and backup/restore page functions with corresponding calls in sts (set terminal state).

July 20th:

- Started working on a manual. Hopefully it won't take forever to complete.
- Started working on a very important feature. Basically, it will permit the calling application to address various special parameters on a per character basis. I'm currently working on adding individual control over character; alpha, zoom, x and y offsets, and finally rotation. When these features will be implemented, it will be possible to make all sorts of dazzling effects and sweeps on a per character basis, much like addressing character color and so on. So, implementing this shouldn't be too much of a stomach burn. But I'll give myself a solid week or two. These are basically what I call the "special features set". It's switchable on/off on a per page basis, for performance control, because those extra features come with a cost to performance, it is very important to let the calling app control these with access to these caps control. Thus the flag is allready in SGR (Set Graphics Rendition). Since the data structure of these are the same as all the other page layers, CSI_fill_layer_zone already works for them. But, the toughest part is modifying render_page()........
- I've expanded the concept of page scrolling, as PCTerm allows scrolling upwards with reverse linefeeds, I had to implement it (better late than never). I've also (again, very late in the doing) implemented TRMOD_WRITE_PROTECT, TRMOD_PROTECT, TRMOD_AUTOPAGE, TRMOD_AUTOSCROLLING, TRMOD_EOL_WRAP, TRMOD_RECEIVE_CR, TRMOD_LOCK_CURSOR_LINE and TRMOD_RECOGNIZE_DEL terminal modes. Half of them are now active flags, the rest of them are yet to be fully implemented in the logic. This is part of an action, to clean up the terminal modes and the logic, as well as cleaning up the ASCII escape sequences.


Polishing the terminal + adding more stuff	2007-06-24
A lot of polishing went in the code these last few days. Now I've got some caps, error codes, and a bunch of other neat constants that I put right in there at use. It looks a lot cleaner. I've also renamed a truckload of functions to something that makes more sense. Fixed a couple of bugs also that was creeping along the various palette, mouse, display manipulation and in the CSI invoker, which I continued expanding with more calls.

I've recoded the page moving system entirely in order to let the system decide more how things should get shuffled. This way I am able to fine grain the page manipulation options, which is something that I had in mind for a while. I've added more caps there in order to decide how pages can get moved, such as bleeding off borders left, right, top, down, or simply able to move the page in x or y. So, this way it is possible to have complete control over every page and let the user move things freely, or choose to have pages that are un-movable. Of course all this is tweakable with CSI arguments. The toughest part that I haven't totally sorted out about this page moving system, is the fact I can have pages stacked one under the other. That is puzzling to generalize such behaviour into simple functions, as I had a lot of special calculations to properly make this work. Hopefully I can get rid of this headache.

I've planed to have the capability of up to 32 pages on screen. But I haven't yet programmed a page focus system. That should be quite a task thoe to program, since you still want to preserve the priorities of the previous pages, and also, some of my pages are pre-programmed in a certain order, which can't be changed. So defining page caps for that is going to be quite a circus act.

I've tought of making a window type management within the terminal, but that won't be necessary, as the page system does exactly that. And it doesn't forbid me to implement pull down menus, eventually.

There are other tricky things I wish to do also, which are adding shadows to my pages. That would be neat actually, as a page pases over another page, it would leave a shadow behind in an isometric angle, about one or two character size off. I suppose the best way would be to just make a black square with an alpha of something like 0.3 or 0.5. This is just speculative, but I'm interested in doing this.

June 27th:

- Fixed again a huge amount of bugs and inconsistencies
- Implemented keyboard character repeat and the parms are all tweakable through CSIs
- Standardized loads of functions, variables, constants
- Added preliminary page shadow system *not yet working*
- Added preliminary page display priorities *not yet working*
- Properly implemented page_focus (previously called current_page)
- Finished implementing strings in the CSI tokenizer
- Added more float handling to the buffers
- Added a font load/clear CSI and standardized font management, to externalize font handling
- Added other CSIs for enabling external access to various page states, which I also standardized
- standardized page display modes
- Fixed character blinking, now the fast blink is dependant on the slow blink, if both are applied

June 28th:

- It is now possible to initialize and shutdown the terminal through the buffers using set_terminal_state.
- Expanded devise_status_report with stuff like report_mouse_status, report_active_pages, ...
- Implemented set_font_state, set_keyboard_state, set_shadow_state in the pipeline
- Generalized page dependencies for split pages
- Added CSI_move_page and CSI_set_blink_rate

Now, the problem is, since I allow to initialize and shutdown the terminal, and leave access to the buffers, of course, I need to safeguard all the graphics functions that are currently not safeguarded. So, that will create a small overhead in the execution, but it will not be possible to notice IMHO. There is always a way to verify for terminal initilialization in the upper layers instead of hundreds of checks. But I'm still a little puzzled about this. But checking for the buffers being initialized wasn't anything earthshattering as I only check where it counts; term_receive_byte() and term_send_byte()!

June 29th:

I've finally come to the point where I can display CSI error codes in the debug mode! I've set up a dozen error codes and they all seem to report. Debug is presently on term page only (not on files), but a debug destination page parameter is there so the information can be sent to any page which becomes a page for debug use. There are 5 levels of debug. Only level 1 and 2 are able to emit return codes. It is meant so that you can have more limited debug information. In most debug modes, all the CSI stuff is shown on the debug page, but I'll modify it right now so in debug mode 5, only the bad CSIs get shown ;).

June 30th:

Finally got the page display order working (400 lines of code later). But it is halfway implemented as it is not yet used as a reference when displaying pages! So this job is halfway done. The list is implemented in the page system, and contains exactly the information needed thoe, which I have verified. I created a debug CSI where I put the commands that dump debug information on the terminal screen :o where I can verify. But there are many bugs... I've encountered completely misleading stuff where I didn't even send the right parms to the right places and it was a big mess. That's what happens when you wake up the next day, you find stuff like that and you wonder how the heck you came up with such inconsistencies.

July 5th:

- Improved debugging
- Fixed bugs in draw list
- Removed page inits from the terminal and put them in the console init procedure as CSIs
- Fixed "add page to draw list" function and it's sub functions so we don't have pages initialized without proper management (return_code = 0) of the draw list.
- From now on, the *only* functions to ever be used from a calling app to the terminal system should now be term_receive_byte() and term_send_byte().

July 6th:

- Rewrote how margins work. Now it makes sense and it is usable. Added CSI_set_margins.
- Simplified the way pages are initialized, a little.
- There was a nasty bug about double/triple/quad size text, where it was massing up the letters (scrambling them!) depending on wether or not and how I was moving the pages. Specially if the page was bleeding off the edges (top and left). So I managed to fix that in a way with minuscule impact on framerate.


Terminal sub-project morphing into game menu sub-project	2007-06-04
As the title says, the Terminal is slowly showing potential for my games menu! I'm slowly implementing a clickable items selector in the terminal module. What this does basically is the following:
- On a mouse over (before click), the system will show a mask at active page when in "mouse over" item. So this permits having a highlighted box around the item which is clickable while the mouse passes over that. Meanwhile, the terminal scans the mouse for a left click.
- If a click is pressed, the terminal picks it up and the accompanying mouse coordinates (page, col, row)
- It then looks in that page to see if there's a clickable item under that mouse pointer. If there is, then it's going in the mask sub-option where a "mouse clicked" mask is applied while the click is being pressed.
- When the mouse click is finally released, the terminal returns an "item # has been clicked" message to the console.

It's a pretty entertaining thing to manage as there are various page setups involved that needs to be supported while this is happening. It is also important that the terminal setup voids all sorts of weird reactions towards the user's mouse actions, and to return the proper visuals from an appropriate termial display assembly.

This scheme is still an on-going development, but it requires a little tought to make sure that the realisation is fully feasible. Meanwhile, I had to adapt the terminal to accept these new none the less interesting features. The creation of mask 1 and mask 2 were part of the key items as it is about visual effects. Well, the casual user probably won't notice the whole trickery going on in the back as he is used to those sometimes entertaining games menu, but it's important to have things visually interesting. If you have a menu and things don't react, say, to your mouse passing over the options, or to the mouse clicks, then it's a little harder to see (from the user's perspective) if the computer knows what he's requesting, or if it reacted according to the controls. So, that's the role of the masks. In simple programmer terms, it's to switch parts of the display according to a "mouse over" and to a "mouse click". I could have tought about more ways to use these masks, but for now I'll try to have the mouse interact with these methods as it should.

I've corrected many bugs and expanded on the VT commands also. Sped up yet again the terminal (passed from 780 fps to 945 fps for a blank screen, which is faster than even the simplest earliest form of the terminal project!). While displaying my test.ans, I noticed it went from 160fps to 167fps (which is a nice little gain).

I've changed my overlay screen from page 4 to page 7, and reserved page 4, 5, 6 for game menus. Page 0, 1, 2, 3 remains console screens.

June 8th:

I've finally come to make the clicking, masking code to work, both mouse, and screen display. I had a bug that was keeping me from advancing on that issue for 2 days, but now it is resolved... I must admit I've been quite tired lately. Both the heat is turning on over in the Montreal region and the humidity is up high. It's getting hot and the sky isn't really shining bright. Anyhow, I'm happy I got that done and now I can move to something else, and put some headaches aside.

There are some other things that need to be done besides being able to click things on the VT screen; I need to be able to receive the clicked item id at the console / calling app! Not only must I send it from the Terminal, but the receiving app must understand what I am sending. But, I have absolutely no tools set up to do so! Yet, I do send streams to calling apps thru a sandard send/receive byte system that I implemented in order to do so. But how do you make a calling app understand what it is receiving? Currently, the calling app is receiving key entries and is handling that on it's own thru my magic gate. But, if I wanted to, I could implement a VT callback function system, but it's pretty hard work, considering how much effort I spend into implementing a full VT/ANSI stream CSI decoder parser. But, apparently, standard VT terminals have the ability to both send and receive CSIs, so I guess I'll have to do that for both directions. In essence, the terminal will have to build it's own CSIs and transcode values for the callback stream, so the calling app can receive and then decode the information. All this brouaha is specifically so that I have the flexibility to send bursts of CSIs and whatnot to whichever direction and have both sides react accordingly to their dialogs. And, that should make for a wonderful little console/terminal world.

June 9th:

- Implemented DSR (Device Status Report), which now can report a bunch of stuff back to the calling application. Most notably, it reports the clicked item. But also, screen resolution, cursor position, mouse position, terminal trace status, terminal split mode, etc. This is how the calling application gets it's info from the terminal, basically. Whatever I need to report back, I can just add more commands here.
- There are effects I wish to implement. I'm trying to figure out where to do this. I've already added the 256 color palette. Also, with VT commands to manage them, and shift palette colors around. But I have other palette tweak ideas. I don't think I need to do fade in/out as this is handled by the alpha of each pages. Maybe morphing one palette into another(?)... Other than palette management, I've tought of character scrolling functions. Appearing / disappearing by various methods. That's something that popped into mind often. If I want to do that, I'll have to locate every character with an X, Y position, angle, etc etc. That is one heck of a task for a terminal. But very feasible. I'm just not sure if it is appropriate. It's a definite performance hit. But I have many tricks that I can use to properly distribute tasks and make this as light as possible on the CPU side.

June 21st:

- Took a big break from coding. But managed to tweak a bit at the terminal. I've basically made the parsers work with banks only, which makes them run much faster now. fixed a batch of bugs, and also improved error checking/correction on the CSI command side.
- I'm considering implementing some window management, which would facilitate using a file selector. With window management, it is possible to not only do a file selector, but a text editor in a window, forms, chat, e-mail, IRC, etc etc... All this is quite easily feasible with pages, just that having the possibility of popping some windows and dragging them with the mouse seems appealing. Like a lot of other ideas, this might go in the bin.


Terminal sub project comming to terme	2007-05-26

Here's a recap of the doings:
- re-implemented underline and strikeout, which are now more efficient and don't take more than 1 or 2 fps overhead when screen is fully busy with those things everywhere
- added a mouse active / inactive flag, so the game application has control over the fact that the user may or may not use a mouse within the terminal
- added multiple cursor and mouse pointer selections. They are mostly various animated graphic pointers that are trigger-able from a CSI argument. This is mostely for slower CPUs.
- Implemented an answerback FIFO queue, which works perfectly.
- Changed keyboard buffering from console to the terminal, which is hooked with the answerback queue.
- Added a flag to control if a user key entry can send CSIs or not (more control over the user form the calling app).
- Implemented a trace system specifically to monitor CSI commands. The trace system dumps bad CSIs and their arguments to a reserved page in the temrinal, which can then be consulded by flipping pages (PGUP PGDOWN).
- Fixed multiple issues with page blanking
- Added a page section copy (a little late in the doing) function and hooked it up to a CSI ( ESC[nP now works), but a block copy is not yet implemented (shouldn't be too puzzling).
- Started implementing protected zones. It works fine, but is not hooked to many things yet.

June 1st:

- Multiple sets of palette definitions are now possible, with the option to rorate palette colors (for some color effects)
- Implemented font double/triple/quad width as well as double/triple/quad height. The usage is not yet simplified.
- Some speedup to page drawing, for a 20-25% speed increase when terminal pages are blank, but only about 2-3% speed increase when page is fully busy.


One more for the terminal sub-project :P	2007-05-24


The screen shows 20 FPS, but that's because I was trying to get a good snapshot and the HDD was running and the file dumping, so it slowed it down. But otherwize, with a busy screen like that, it's about 50 fps on my machine. But this screen is real busy! 4 pages, the 2 on the right are shifted up a bit. One has an alpha value of 0.6. You can see one of my finished test patterns, and another one also.

Anyhow, it's taking shape.


Still more Terminal emulation work	2007-05-23
Here's a recap of the last bits I've found time to do:
- Fixed a newline bug which now permits to have fullscreen text without a newline moving everything up and killing the top line. I've made it like how it works in the ANSI DOS terminal.
- Fixed a bug in cursor positioning, which in occurence was a bug in the CSI tokenizer.
- Added one HOME CSI as it was missing. Fixed a couple of ANSI arts by doing so.
- Added some ANSIPLUS functions and color set
- Implemented the Conceal, underline and strikeout attributes
- Implemented 256 color palette, which works fine. Every page has it's own palette setting.
- Created a semi-standard test pattern in the Console. It looks a bit like those test patterns you used to see in the early 90s on arcade machines (a grid, RED GREEN BLUE B&W shade, some other bits)
- Fixed all blanking requests so it also blanks background colors. This fixes a couple of ANSI arts that I use as tests, most notably the "Hidden Empire" lemmings ANSI art.
- Tiny speedups in the display, nothing major. I tried various ways to increase the speed of the display with little success. I believe I've done best I could with the current way it's managed. My hopes are to increase the speed of the terminal display to a point where it would be a negligible overhead towards the 3D application. Right now, it's rather hungry IMHO. The content is being refreshed every flip, and currently I have no choice but to work this way. But I have the infrastructure in place to update "when it is needed". Just missing some bits about the vertexes.

Things that will soon make it:
- Another test pattern, this one will be just like the usual one you see on television. It's the one giving the color bars. It's basically done using ANSI codes, with some of my in-house codes.
- Implement protected zones
- Start implementing menus, clickable page sections. This will be used for my editors.
- Make a very tiny text editor in the Console. This is very important, because then I'll be able to implement an e-mail sending system. The reason why I wish to send e-mails from the console is rather simple. Say the game goes to a beta tester, and he finds a bug. A pre-formated e-mail can be accessed, and all the beta tester has to do is fill in the form. Nothing really fancy, but something that would help out, which is the goal of this exercise.

Meanwhile, the terminal works marvels, with possibility of 4 pages displayed at the same time, or 2 pages with screen split horizontally, or vertically. Pages can be scrolled up to reveal 3D in the back. An alpha level can be set to show through the pages. Several fonts are accessible, which is great.

But overall, it's a relatively simple and small sub-project that hopefully will be put aside in the near couple of days as I put my energy more into the real project which is the game.


More Terminal emulation work	2007-05-18
I've always wanted to build my own terminal emulation system. I think I've done a good job already. But it's still not complete. Here's a short recap:
- Installed partial VT52, VT100, VT220, VT420 and VT500 escape sequences (CSI requests)
- Implemented pretty much all of the Set Graphics Rendition command set + many of my own design
- Implemented some of my own VT standard
- Implemented Multiple page system similar to VT420 but so different that I made it my own command chain
- Terminal display can be split horizontally, vertically, for up to 4 pages at once, each with their own character resolutions.
- Every page can use a different color palette, but it is a 16 color palette still (not like newer VT standards)
- Pages can be moved vertically using the mouse. So there is a mouse pointer on the screen.
- The mouse pointer and key entry cursor are all fully programmable with an animation sequence for both color and character.
- Implemented PAGE 4, which is what I call an immediate page. It serves as a trace display or prompter, that is an overlay of the terminal. It is always active, so a trace of anything anywhere in the game project is possible with this page.
- Implemented font system that permits up to 8 (not 10 like the VT standards permit) different fonts in terminal memory. It is possible to have all 8 fonts displayed on each page, without any limitation. Reason for 8 fonts instead of 10 is specially because I had some needs for home made rendition commands that I placed in the same sequence numbers.

Currently, the refreshes that I am getting aren't really great (about 220 FPS for a full screen of text, 160 FPS for a complex ANSI art with various color use, 80 FPS for 4 pages at once with various things on each pages). It is hoped I will bump the refresh rate up, but I am suspicious

Things that aren't implemented:
- Choose between font double width, double height. But I wanted also triple and quadruple width and height. Not sure if I will... It will break a lot in the current implementation of the font system if I try to put this.
- having pages bigger than the screen. I wish I could implement this, but it's quite complex to do, specially now with the possibility to split the terminal display in various ways and scroll the display with the mouse...
- Having 256 color choice would be really nice. And I wish I could implement that, but it would break all of the color bank mechanism, which would double it's size. It would actually be simpler to implement true color that implementing a 256 color palette... A 16 color palette (current version) works wonders as 1 byte contains foreground and background color.

Now it's very important for me to mention that, I firmly believe that I haven't overdone this. I think many game developper would wish they had taken the time to make a terminal system for their game, so they can monitor development versions with various traces, log management, INI file requests, script trace, etc etc. I think it's just a blessing to have this and I'll stick to those principles. I specially enjoy the possibility of having PAGE 4 as it will really add to ways that are possible to follow basic things, such as; fps, tri count, various flags, entity positions, and so on.


Small advances	2007-05-12

- Worked on the tokenizer / requester
- Worled on my commands list and components list
- Sped up the terminal display (a full page of text and various colors went from 110 fps to 170 fps, and an empty page went from 200 fps to around 900 fps. It can still be improved, in due time)
- Added a mouse pointer and mouse Y screen control to the terminal display, so now the mouse can slide the terminal display up and down to make way for the 3D behind it.
- Re worked the console so it can send commands to a requester (not yet completed)

TODO:
- Complete the requester ***
- Integrate command & component requester to the console ***
- Integrate the console to the game engine **
- Build a game menu (with sub menus, specially the one with keyboard and mouse controls)
- Hook up components settings with game engine *
- Create a game ini file management *
- Create savegame file management
- Convert grid object builder to work with requester
- Convert procedural texture builder to work with requester

* = priority


Finally starting to show some development	2007-05-10
Hello!

Well, let's present the project. This will be a game, where you control a sphere around a 3D world, a little like what you can see in the game Marble Madness. There will be tons of places to go and puzzles to solve to advance to various areas of the world. The game world will be segragated in various zones. Every zone will be accessed via flying through tunnels. The player will be involved in various surface places, and also some underground places. When travelling in flying mode, a ship will be supplied which can also travel under water.

What has been done so far:
- Basic 1st + 3rd person 3D game engine has been written
- All controls are customized for mouse / keyboard action
- Implemented Newton Physics, hooked to all objects + player + bots
- Implemented Ashadow and some advanced water techniques (aka HL2 reflect/refract/fog)
- Created terrain, model generating engine
- Created procedural texture generating engine
- Created a Console to access the game engine
- Created a text Terminal that is an overlay of the 3D scene
- Created player models (textured spheres basically, with the possibility of either real time cube mapping or sphere mapping)
- Created parsing commands for the model engine (done, but will be expanded)
- Created parsing commands for the texture engine (done, but will be expanded)
- Created Terminal system, highly influenced by various VT standards
- Implemented a console system that will serve for various background work and possibly a game editor

To be implemented:
- Create a Manager engine
- Create an Event engine
- Create a Scene engine (aka zones)
- Fix shadowing problems (some day it may work)
- Fix performance issues when using cubemaps
- Implement a mesh optimizer
- Implement a mesh sectioning algorythm
- Add displacement mapping (maybe, but sketchy ideas)
- Add specular mapping (maybe, but maybe not)
- Replace Newton physics by PhysX (some day, for sure)
- Define what the ships will look like (soon)
- Define some animated creatures
