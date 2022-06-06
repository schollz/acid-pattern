# acid-breaks

this script is meant to turn the norns into an "[acid breaks](https://electronicmusic.fandom.com/wiki/Acid_Breaks)"-style groovebox. 



## about


I wanted to make a 303-esque thing and at the heart its very simple - a resonant filter with a saw/square wave. the tricky bit is having the right "accent", the accent envelope circuit affects the amplitude and the filter opening. apparently its not a typical envelope generator - it has a capacitor that doesn't completely discharge allowing you to get "higher" as you add more accents. its well described [in this classic article](https://www.firstpr.com.au/rwi/dfish/303-unique.html):

![article](https://user-images.githubusercontent.com/6550035/172177968-8fa32b81-0b3b-4ae7-a253-93f263fcdd7f.png)

it took a bit but I figured out I could recapitulate this exact behavior using a single bus with multiple envelope generators in SuperCollider:

![supercollider](https://user-images.githubusercontent.com/6550035/172177717-bbd0b5ca-ab4c-4dff-b3eb-4199df110f84.PNG)

<details><summary>sc code</summary><pre>b=Bus.control(s);
c=Bus.control(s);
(
{
	Out.kr(0,EnvGen.ar( Env.new([0, 1, 0], [0.04, 4],-8),TDelay.kr(Impulse.kr(0),0.08),doneAction:2));
}.play;
{
	Out.kr(1,EnvGen.ar( Env.new([0, 1, 0], [0.04, 4],-8),TDelay.kr(Impulse.kr(0),0.08),doneAction:2));
}.play;
{
	Out.kr(0,EnvGen.ar( Env.new([0, 1, 0], [0.04, 4],-8),TDelay.kr(Impulse.kr(0),0.3),doneAction:2));
}.play;
{
	Out.kr(0,EnvGen.ar( Env.new([0, 1, 0], [0.04, 4],-8),TDelay.kr(Impulse.kr(0),0.6),doneAction:2));
}.play;
{
	[In.kr(1),In.kr(0)];
}.plot(2);
)</pre></details>


## install

install mx.samples2
```
;install https://github.com/schollz/mx.samples2
```

get the nessecary patch

```
os.execute("mkdir -p ~/dust/audio/mx.samples/ultra_synth")
os.execute("wget https://github.com/schollz/mx.samples/releases/download/samples/ultra_synth.zip -P /tmp/")
os.execute("unzip /tmp/ultra_synth.zip -d ~/dust/audio/mx.samples/ultra_synth/")
```

install tapedeck

```
os.execute("cd /tmp && wget https://github.com/schollz/tapedeck/releases/download/PortedPlugins/PortedPlugins.tar.gz && tar -xvzf PortedPlugins.tar.gz && rm PortedPlugins.tar.gz && sudo rsync -avrP PortedPlugins /home/we/.local/share/SuperCollider/Extensions/")
```

install acid-breaks

```
;install https://github.com/schollz/acid-breaks
```


## todos

- ~~lfos for each parameter (depth and period)~~
- ~~clamps on all the variables in SuperCollider~~
- ~~duration working~~
- ~~fix amen cleanup~~
- ~~add scale changes? root note changes?~~
- ~~recover sequence on save~~
- use toggles instead of triggers for lfos: TODO test
- stutter fx (gated etc): TODO test
- ~~allow pattern to draw itself~~
- ~~allow multiple patterns~~
- add more chaotic lfos
- ~~add start and stop buttons~~
- ~~add start and stop transport~~
- allow using other samples for the pad
- allow 16 tracks
