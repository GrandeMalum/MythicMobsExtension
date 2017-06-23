# CustomSkillMechanics
for MythicMobs 4.0.1 and higher

#### *** 22.6.2017 *** added targetinsight option to customteleport mechanic. destination targeter now parse variables. Changed sdelay to teleportdelay. See customteleport for more details.
#### *** 21.6.2017 *** added some more improvements to customteleport. See customteleport for details and example.
#### *** 20.6.2017 *** added setowner boolean to customsummon. See customsummon for details.
#### *** 14.6.2017 *** added customteleport mechanic. See customteleport for details. Beta in meanwhile. Released in hurry.
#### *** 14.6.2017 *** added ranged amount to customdamage. See customdamage mechanic for details.
#### *** 11.6.2017 *** added temporary biome condition fix for randomspawners with spawn method ADD. See biomefix for details.
#### *** 10.6.2017 *** added dropmythicitem skill. See dropmythicitem mechanic for details.
#### *** 26.5.2017 *** added ignoredestoffsetyaw (idoy) to customparticleline. See customparticleline mechnic.
#### *** 25.5.2017 *** added customparticleline & added keeptargetyaw and keepcasteryaw to swap mechanic. See customparticleline and swap mechanic for details.
#### *** 24.5.2017 *** added percentage to customdamage. See customdamage for details.
#### *** 24.5.2017 *** added setthreattarget & clearthreattarget mechanics.
#### *** 24.5.2017 *** added swap mechanic.
#### *** 20.5.2017 *** added useEyeDirection and inFrontBlocks attributes to customsummon mechanic.
#### *** 20.5.2017 *** finally fixed all attribute in mobsinradius.
#### *** 19.5.2017 *** added stun mechanic & isstunned condition.
#### *** 19.5.2017 *** added CustomSummon mechanic. Usage same as the original MythicMobs summon but in addition you can use addx(ax),addy(ay),addz(az) to the location. 
#### *** 19.5.2017 *** fixed casesensitive for all in mobsinradius condition.
#### *** 9.5.2017 **** activated MythicMobs ~onKill trigger for all ActiveMobs (see example at the end of the readme)
#### *** 8.5.2017 **** added lastdamagecause condition.
#### *** 6.5.2017 **** merged with CustomConditions & added mobsinradius condition.
#### *** 30.4.2017 *** added removepotion, feed & oxygen mechanics.
#### *** 26.4.2017 *** fixed issues in customdamage. now that skill work how it should.
#### *** 23.4.2017 *** added customdamage mechanic as a temporary fix for damage mechanic.
#### *** 7.4.2017 **** fixed random bug in damagearmor and added support for negative values. Because of that changed ranged syntax from "1-2" to "1to2"



# Mechanics:




## customteleport skill:
	
	Advanced teleport mechanic. Use this to teleport from/to variable destinations with variable behaviors.
	Options:	destination= MythicMobs targeter or vanilla targeter. Use "" that the targeter can be parsed.
				noise=n= number, random point around the target
				teleportdelay=tdelay=td= number, delay in ticks between teleportation (if more than 1)
				infrontof=front=f= true/false, teleport in front of target (if target is a entity)
				returntostart=return=r= true/false, if the source entity should return to its start position
				betweenlastentitysignal=bls= signalname to be send to caster mob between the teleportations, where the trigger is the last entity (if target was an entity)
				betweennextentitysignal=bns= signalname to be send to caster mob 
				ignoreowner=io= true/false, if the owner of the caster mob should be ignored.
				maxtargets=mt= number, the maximium number of targets.
				targetinsight=insight=is= true/false, only the targets insight of the current position are used.
```
Example Mob:

Monkey:
  Health: 300
  Type: zombie
  Display: "Monkey"
  AIGoalSelectors:
  - 0 clear
  Skills:
  - customsummon{t=ChainDummy;setowner=true} @selflocation ~onDamaged
  
ChainDummy:
  Type: armor_stand
  Options:
    Invisible: true
    Invincible: true
  Skills:
  - customteleport{destination="@EIR{r=40}";teleportdelay=10;front=false;fs=ende;bns=bns;bls=bls;r=false;io=true;is=true} @self ~onSpawn
  - skill{s=chain} @trigger ~onSignal:bns
  - remove @self ~onSignal:ende
  
#  NOTICE THE "" FOR THE DESTINATION!

Example Skill:

chain:
  Skills:
  - lightning
  - customparticleline{particle=reddust;amount=5;color=#feff90;ys=2.5;vd=1.0;hd=-0.5;distanceBetween=0.5;tyo=1.25}
  
  
Or use some variables in the destination targeter:

  - customteleport{destination="@Location{c=<mob.l.x>,<mob.l.y>,<mob.l.z>}"} @eir{r=40} ~onDamaged
  
```
	
	
	
## dropmythicitem skill:
	
	Drop a mythicitem or a mythicdroptable.
	Options: mythicitem=item=itemtype=type=t=i= Internal name of the mythic item or the mythic droptable.
	         amount=a= the amount of the that will be dropped if a mythic item, the amount of droptable creation if a droptable.
	
	- dropmythicitem{item=MythicItem;a=1} @pir{r=20} ~onAttack
	
	This drop one MythicItem at all players in radius of 20 if the mob targets a entity.



## customparticleline skill:

	The same like the original but added vDestOffset & hDestOffset to adjust the targetlocation.
	Use vDestOffset to adjust the height and use hDestOffset to adjust the x/z position. Use idoy true/false(default) = ignoredestoffsetyaw to ignore the yaw of the targetlocation. 
	With this its possible to target a specific part of the entity.
	
    - customparticleline{particle=reddust;amount=5;color=#feff90;ys=2.5;vd=1.0;hd=-1.0;idoy=true;distanceBetween=0.5;tyo=1.25} @pir{r=10} ~onTimer:5



## setthreattarget & clearthreattarget skill:

	Use setthreattarget to clear the activemobs threattable and add {a=double} targeter to the threat. Default amount is 65536.
	
	- setthreattarget{a=11111} @p
	
	To clear the threattable and trigger the dropCombat Event use:
	
	- clearthreattarget @self



## SwapSkill:

	Swap location of caster and target. Use keeptargetyaw=kty false/true or keepcasteryaw=kcy false/true to keep the original direction or not.
	
	- swap{kty=true;kcy=false} @target



## StunSkill:
	
	Use this mechanic to stun the target. 
	
	- stun{duration=Ticks} @target
	
	Where duration=d=how many ticks the target will be stunned. In addition there is the isstunned condition. Look at Conditions.
	


## CustomSummonSkill:

	Use this mechanic to add ax,ay,az to the targetlocation.
	
	- customsummon{type=WITHER_SKELETON;amount=5;radius=4;ax=5;ay=1;az=2} @self
	
	This example do not check for a safeplace it will spawn the mob no matter if its in block or such. If you want to use safeplace add noise. Example:
	
	- customsummon{type=WITHER_SKELETON;amount=5;radius=4;noise=1;ax=5;ay=1;az=2} @self
	
	This example shows how to spawn mobs relative to the direction of the targeted mob:
	
    - customsummon{t=mobname;ued=true;ifb=1} @self
	
	ued=useEyeDirection=EyeDirection;ifb=inFrontBlocks=inFront
	
	Use setowner (true/false) to set the owner to the mob which casted the custumsummon skill.
	
	- customsummon{t=mobname;setowner=true} @selflocation
	
	This summon the mob mobname and set its owner to the mob which casted the skill.



## OxygenSkill:

	Use this mechanic to add oxygen to the LivingEntity.
	
	- oxygen{a=20} @self
	
	a = amount = the amount of oxygen that will be added. A player has a usual max air amount of 300. A amount of 20 gives about 1 bubble air.
	
	
	
### FeedSkill:

	Add specific amount of food to the players foodlevel.
	
	- feed{a=1} @PIR{r=20}
	
	a = amount = the food amount. The amount of 1 add 1/2 foodlevel.



## RemovePotionSkill or CureSkill:

	Use this mechanic to remove all or a specific potion from the targeted entity. Use removepotion or cure.

	- cure{t=ALL} @self
	- cure{t=INVISIBILITY} @PIR{r=30}
	
	- removepotion{t=ALL} @self
	- removepotion{t=INVISIBILITY} @PIR{r=30}
	
	t = type = p = potion = ALL by default. Use "ALL" to remove all effects or use a single effect or a list like: p=INVISIBILITY,SPEED,HEAL

	

## CustomDamageSkill:

	Use this mechanic for a temporary mythicmobs damage mechanic fix. See http://www.mythicmobs.net/index.php?threads/skill-damage-bypassed-armour.3373/ this thread for details of the issue.
	
	- customdamage{a=1to2;ia=false;pk=false;pi=false;iabs=false}
	
	a = amount = 1 by default. The amount of damage applied. 1 = 1/2 heart. Ranged amount possbile: 1to2
	ia = i = ignorearmor = false by default. If the armor of the target should be ignored or not.
	pk = pkb = preventknockback = false by default. If knockback should be applied or not.
	pi = preventimmunity = false by default. If immunity should be used or not.
	ignoreabs = iabs = false by default. If absorbation should be ignored or not.
	percentage=p=false by default. If true it uses the amount as percent.
	pcur=pc=false by fault. Need that percentage=true. If pc is true is use the percent of current health if false it use percent dmg of maxhealth.
	

	

## EquipSkullSkill:

	This is a fix for custom player heads for 1.8.8 as it appeard to not work with mm versions greater than 2.5.1
	- equipskull{skull=mythicitemname}
		skull or s: and valid MythicMobsItem to equip on head.
		
Example:
```
mob yml:

Monkey:
  Type: zombie
  Display: 'a MythicMobs Monkey'
  AIGoalSelectors:
  - 0 clear
  Skills:
  - equipskull{skull=Pirate} @self ~onSpawn 1
  
item yml:

Pirate:
  Id: 397
  Data: 3
  Options:
    SkinTexture: eyJ0ZXh0dXJlcyI6eyJTS0lOIjp7InVybCI6Imh0dHA6Ly90ZXh0dXJlcy5taW5lY3JhZnQubmV0L3RleHR1cmUvZmVmMDEyOTdlMmUxYWNkMDQ4ODJhMGM0NGU0OGYxZjE1Y2JiYTI1ODJmOTFiMDgxYzkyOTIwZmVkOGYzMjIwIn19fQ====
```


## DamageArmorSkill:

	- damagearmor{armor=<all>||<list>||<single>;damage=<int>||<ranged>;signal=<string>}
		armor: all / helmet / chest / leggings / boots / hand / offhand - can be single name or all or a list
		damage: armor damage amount as integer or as ranged value for ranged value use "to".
		signal: name of the signal which should be send to mob if an armor part is broken.
		        dont set it if you dont want a signal to be send.
				
Example:
```
dmgpig:
  Type: pig
  Display: 'DontHitMe'
  Health: 500
  Damage: 1
  Faction: neutral
  Despawn: true
  AIGoalSelectors:
  - 0 clear
  - 1 randomstroll
  - 2 float
  Skills:
  # this will damage the whole armor and handitem & offhanditem with damage value of 20
  - damagearmor{armor=all;damage=20;signal=armorbroken} @target ~onDamaged >0 1
  # this will damage only the chest with a random value between 1 to 20
  - damagearmor{armor=chest;damage=1to20;signal=armorbroken} @target ~onDamaged >0 1
  # this will repear hand,offhand,helmet items with a random value between 20 to 40
  - damagearmor{armor=hand,offhand,helmet;damage=-20to-40;signal=armorbroken} @trigger ~onDamaged >0 1
  - message{msg="Oh dear! A part of your armor is gone!"} @trigger ~onSignal:armorbroken
```


## GrenadeSkill:

	- grenade{size=3;amount=1;fuse=20;fire=false;breakblocks=true;utime=60;ueffect=true;undotnt=true}
		size: radius of the explosion
		amount: how many grenades the mob throw at once
		fuse: how long the fuse tick
		fire: should there be fire too? true / false
		breakblocks: damage the blocks? true / false
		utime: how many ticks until restore the blocks
		ueffect: should there be a restore effect? true / false
		undotnt: should the blocks be restored at all? true / false

Example:
```
grenadezombie:
  Type: zombie
  Health: 20
  Skills:
  - grenade{size=2;amount=1;fuse=20;fire=false;breakblocks=true;utime=40;ueffect=true;undotnt=true} @target ~onTimer:200 1
```


## SetRandomLevelSkill:

	- setrandomlevel{min=1;max=10;self=true}
		min: lowest level
		max: highest level
		self: true = targetself / false = target any other targeted mob
		
Example:
```
RndLvlMob:
  Type: zombie
  Skills:
  - setrandomlevel{min=1;max=10;self=true} @self ~onSpawn 1
```


## StealSkill:

	- steal{items=ITEM:AMOUNT,ITEM:AMOUNT,.....;failsignal=steal_fail;oksignal=steal_ok}
		items: Can be a list of valid spigot items. One of the items shuffled by random will be tried to steal from the targeted player.
		failsignal: name of the signal that should be send to the mob if the stealing failed. default signal = steal_fail
		oksignal: name of the signal that should be send to the mob if the stealing was good. default signal = steal_ok
		
	- DropStolenItems
		use this skill and the mob drop all its stolen items. Good idea to use it on death. Or all the items are gone with the mob to herobrine.
		
	There is a buildin cancel damage condition if the mob should make no damage while try to steal. It can be activated if the stance of the mob is "gostealing"
	So if the mob have the gostealing stance set, it will do no damage to its target.

Thief example:

mob yml:
```
thief:
  Type: villager
  Display: 'Thief'
  Health: 20
  Damage: 0
  Modules:
    ThreatTable: true
  Options:
    AlwaysShowName: false
    Despawn: true
  AIGoalSelectors:
    - 0 clear
    - 1 meleeattack
    - 2 avoidskeletons
    - 3 avoidzombies
    - 4 randomstroll
    - 5 float
  AITargetSelectors:
    - 0 clear
    - 1 players
  Skills:
    - setstance{stance=gostealing} @self ~onSpawn 1
    - skill{s=FleeGotSomeStuff;sync=true} ~onSignal:steal_ok 1
    - skill{s=FleeButGotNothing;sync=true} ~onSignal:steal_fail 1
    - skill{s=Steal} ~onTimer:60 >0 1
    - DropStolenItems ~onDeath 1
```
skillfile:
```
Steal:
  Cooldown: 1
  TargetConditions:
  - distance{d=<3} true
  - lineofsight true
  Conditions:
  - stance{s=gostealing} true
  Skills:
  - steal{items=DIAMOND_SWORD:1,IRON_SWORD:1,DIAMOND:3,EMERALD:3;failsignal=steal_fail;oksignal=steal_ok} @NearestPlayer 0.75
  
FleeGotSomeStuff:
  Cooldown: 1
  Skills:
    - setstance{stance=flee} @self
    - RunAIGoalSelector{s=clear}
    - delay 2
    - RunAIGoalSelector{s=fleeplayers}
    - effect:smoke @self
    - potion{type=SPEED;duration=200;level=1} @self
    - delay 400
    - effect:smoke @self
    - remove @self
  
FleeButGotNothing:
  Cooldown: 1
  Skills:
    - setstance{stance=flee} @self
    - RunAIGoalSelector{s=clear}
    - delay 2
    - RunAIGoalSelector{s=randomstroll}
    - delay 400
    - effect:smoke @self
    - remove @self
```



# Conditions



```
  Conditions:
  - biomefix{biome=PLAINS,DESSERT;action=true}
```
Check if the target is in a certain biome.
biome=b= A list with valid biome names.
action=a= true / false
```
  TargetConditions:
  - isstunned{a=false}
```
Check if the target is stunned (true) or not (false) The example will match if the target isnt stunned. Please notice that this can be used at the caster, or as TargetConditions.
If used as TargetConditions the targeter for the metaskill is important.
```
  Conditions:
  - lastdamagecause{cause=ENTITY_ATTACK,PROJECTILE,FIRE;damager=PLAYER,ZOMBIE,SKELETON;action=TRUE}
```
Check what caused the last damage to the MythicMob mob. cause and damager can be a single value or a List
cause=c= The cause of the last damage. Valid is "ANY" or Bukkit's DamageCause https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/event/entity/EntityDamageEvent.DamageCause.html
damager=attacker= The EntityType of the attacker. Valid is "ANY" or Bukkit's EntityTypes https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/entity/EntityType.html
```
  Conditions:
  - mobsinradius{mobtypes=mythicmobs1,mythicmobs2,mythicentity1,mythicentity2;a=5to10;r=20;action=TRUE}
```
Use this condition to check how many mobs are in radius.
mobtypes=types=mobs=mob=t=m= The mythicmobs or mythicentities to check. Or use ALL for any mythicmobs mob.
amount=a=ranged value to match. example: a=<20 or a=>10 or a=5 or a=5to10 for range
radius=r=radius to check
action=true/false
```
  Conditions:
  - wgstateflag{flag=mob-spawning;action=false}
  - wgstateflag{flag=pvp;action=true}
  - wgstateflag{flag=creeper-explosion;action=false}
  .....
``` 
This condition can be used on every allow/deny flag. If region has no flag set, it inherits the flag of the parent region.

```
  Conditions:
  - wgdenyspawnflag{types=zombie,skeleton;action=true}
```
This condition can be used to check if the region denys the spawning of some entitytypes. If region has no flag set, it inherits the flag of the parent region.

```
  Conditions:
  - factionsflag{flag=monsters;action=true}
```
This condition can be used to check if the faction has a specific flag set to true or false. Here is a list of all flagnames: animals, monsters, peaceful, endergrief, explosions, firespread, friendlyfire, infpower, offlineexplosions, open, permanent, powergain, powerloss, pvp, zombiegrief

```
  Conditions:
  - hastarget{action=true}
```
This condition meets if the mob has a target (true) or no target (false).

```
  TargetConditions:
  - vdistance{d=2to3;action=true}
```
This condition checks for the vertical distance between target and mob. Use ">" for greater "<" smaller or "to" for range.

Example:
```
Mobfile:

Monkey:
  Type: zombie
  Display: "&cMythicMobs Monkey"
  Damage: 1
  Health: 10
  Armor: 1
  DamageModifiers:
  - FALL 0.25
  Skills:
  - skill{s=jumpToTarget} @target ~onTimer:50 1
  
Skillfile:

jumpToTarget:
  Conditions:
  - hastarget{a=true}
  TargetConditions:
  - distance{d=<4} true
  - hdistance{d=2to3;a=true}
  Skills:
  - leap{velocity=120} @target
```

#### ~onKill trigger with lastdamagecause condition example

```
BowMonkey:
  Type: skeleton
  Display: "&cBowMonkey"
  AITargetSelectors:
  - 0 clear
  - 1 monsters
  Skills:
  - skill{s=DamageCauseMeele} @self ~onDamaged
  - skill{s=DamageCauseProjectile} @self ~onDamaged
  - skill{s=KillCauseProjectile} @trigger ~onKill
  - skill{s=KillCauseMeele} @trigger ~onKill

MeeleMonkey:
  Type: zombie
  Display: "&cMeeleMonkey"
  AITargetSelectors:
  - 0 clear
  - 1 monsters  
  Skills:
  - skill{s=DamageCauseMeele} @self ~onDamaged
  - skill{s=DamageCauseProjectile} @self ~onDamaged
  - skill{s=KillCauseProjectile} @trigger ~onKill
  - skill{s=KillCauseMeele} @trigger ~onKill

KillCauseProjectile:
  TargetConditions:
  - lastdamagecause{cause=PROJECTILE;damager=ANY;action=TRUE}
  Skills:
  - message{msg="Sorry <trigger.name> i am no Wilhelm Tell though!"} @world
  
KillCauseMeele:
  TargetConditions:
  - lastdamagecause{cause=ENTITY_ATTACK;damager=ANY;action=TRUE}
  Skills:
  - message{msg="I killed <trigger.name> just with my hands only!"} @world
  
DamageCauseProjectile:
  Conditions:
  - lastdamagecause{cause=PROJECTILE;damager=ANY;action=TRUE}
  Skills:
  - message{msg="Someone try to shoot me down!"} @world
  
DamageCauseMeele:
  Conditions:
  - lastdamagecause{cause=ENTITY_ATTACK;damager=ANY;action=TRUE}
  Skills:
  - message{msg="Help! It punch me in my face!"} @world
```