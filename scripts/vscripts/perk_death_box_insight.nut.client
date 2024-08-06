
global function Perk_DeathBoxInsight_Init



global function Perk_DeathBoxInsight_IsDeathBoxInsightEnabled
global function DeathBoxInsight_UpdateLookatPing
global function DeathBoxInsight_GetCurrentPingInfo

global struct DeathBoxPingInfo
{
	entity deathbox
	entity loot
}

struct
{
	table<int, bool functionref( entity, entity, LootData )> passiveToShouldSeeFunction
	var deathboxRui
	entity currentLookat
	float lookatAlpha
	int currentLookatIndex
	int currentLookatLootIndex
	int numSlotsInCurrentLookat
	bool isActive
}file

const float MAX_VISIBLE_DISTANCE = 25 * METERS_TO_INCHES
const float MAX_PING_DISTANCE = 14 * METERS_TO_INCHES
const float ICON_UP_OFFSET = 40.0
const float ALPHA_LERP_MULTIPLIER = 5.0
const float PING_DEGREES = 3
const float LOOKAT_DEGREES = 30
const int MAX_DEATHBOX_ICONS = 3


void function Perk_DeathBoxInsight_Init()
{

		if ( !Perk_DeathBoxInsight_IsDeathBoxInsightEnabled() )
			return

		RegisterSignal( "DeathBoxInsightPassiveChanged" )

		AddShouldSeeEntityCallbackForPassive( ePassives.PAS_AMMUVISION, Ammuvision_ShouldSeeLoot )
		AddShouldSeeEntityCallbackForPassive( ePassives.PAS_HEALING_ITEM_VISION, HealingItemVision_ShouldSeeLoot )
		AddShouldSeeEntityCallbackForPassive( ePassives.PAS_ORDNANCE_HIGHLIGHT, OrdnanceHighlight_ShouldSeeLoot )
		AddShouldSeeEntityCallbackForPassive( ePassives.PAS_ULTACCEL_HIGHLIGHT, UltAccelHighlight_ShouldSeeLoot )

		AddCallback_OnPassiveChanged( ePassives.PAS_AMMUVISION, OnPassiveChanged )
		AddCallback_OnPassiveChanged( ePassives.PAS_HEALING_ITEM_VISION, OnPassiveChanged )
		AddCallback_OnPassiveChanged( ePassives.PAS_ORDNANCE_HIGHLIGHT, OnPassiveChanged )
		AddCallback_OnPassiveChanged( ePassives.PAS_DEATHBOX_BATTERY_COUNT, OnPassiveChanged )
		AddCallback_OnPassiveChanged( ePassives.PAS_ULTACCEL_HIGHLIGHT, OnPassiveChanged )



	PerkInfo ammuvision
	ammuvision.perkId = ePerkIndex.AMMUVISION

	if ( Ammuvision_ClassPerk_Enabled() )
	{
		ammuvision.activateCallback = OnActivate_Ammuvision
		ammuvision.deactivateCallback = OnDeactivate_Ammuvision
	}

	Perks_RegisterClassPerk( ammuvision )

}


void function OnActivate_Ammuvision( entity player, string characterName )
{






}

void function OnDeactivate_Ammuvision( entity player )
{






}

bool function Ammuvision_ClassPerk_Enabled()
{
	return Perks_S22UpdateEnabled() && GetCurrentPlaylistVarBool( "perks_ammuvision_enabled", true )
}



void function OnPassiveChanged( entity player, int passive, bool didHave, bool nowHas )
{
	if ( player != GetLocalViewPlayer() )
		return

	if ( !didHave && nowHas )
	{
		DeathBoxInsight_CreateInWorldMarker()
		thread ListenForPlayerDeath( player )
		file.isActive = true
	}
	else if ( didHave && !nowHas )
	{
		player.Signal( "DeathBoxInsightPassiveChanged" )

		if( file.deathboxRui != null )
		{
			RuiDestroy( file.deathboxRui )
			file.deathboxRui = null
		}

		file.isActive = false
	}
}

void function ListenForPlayerDeath( entity player )
{
	player.EndSignal( "DeathBoxInsightPassiveChanged" )
	player.WaitSignal( "OnDeath" )
	if( file.deathboxRui != null )
	{
		RuiSetFloat( file.deathboxRui, "alphaTweak", 0 )
	}
}

bool function Perk_DeathBoxInsight_IsDeathBoxInsightEnabled()
{
	return !( GetCurrentPlaylistVarBool( "disable_death_box_insight", false ) )
}

void function AddShouldSeeEntityCallbackForPassive( int classId, bool functionref( entity, entity, LootData ) callback )
{
	file.passiveToShouldSeeFunction[classId] <- callback
}

bool function OrdnanceHighlight_ShouldSeeLoot( entity loot, entity player, LootData lootData )
{
	Assert( player.HasPassive( ePassives.PAS_ORDNANCE_HIGHLIGHT ), "Player has the wrong passive for this!" )

	switch( lootData.ref )
	{
		case GRENADE_EMP_WEAPON_NAME:
		case "mp_weapon_thermite_grenade":
		case "mp_weapon_frag_grenade":



			return true
	}
	return false
}

bool function UltAccelHighlight_ShouldSeeLoot( entity loot, entity player, LootData lootData )
{
	return lootData.ref == "health_pickup_ultimate"
}

bool function Ammuvision_ShouldSeeLoot( entity loot, entity player, LootData lootData )
{
	Assert( player.HasPassive( ePassives.PAS_AMMUVISION ), "Player has the wrong passive for this!" )

	for( int i=0; i <= WEAPON_INVENTORY_SLOT_PRIMARY_1; i++ )
	{
		entity weaponAtSlot = player.GetNormalWeapon( i )
		if( !weaponAtSlot )
			continue


		
		if( !weaponAtSlot.GetWeaponSettingBool( eWeaponVar.uses_ammo_pool ) )
		{
			continue
		}
		else if( weaponAtSlot.GetWeaponBaseClassName() == "mp_weapon_car" )
		{
			if( lootData.ref == AmmoType_GetRefFromIndex( eAmmoPoolType.bullet )|| lootData.ref == AmmoType_GetRefFromIndex( eAmmoPoolType.highcal ) )
				return true
		}
		else
		{
			int ammoType = weaponAtSlot.GetWeaponAmmoPoolType()
			string ammoTypeRef = AmmoType_GetRefFromIndex( ammoType )
			if( lootData.ref == ammoTypeRef )
				return true
		}

	}

	return false
}

bool function HealingItemVision_ShouldSeeLoot( entity loot, entity player, LootData lootData )
{
	Assert( player.HasPassive( ePassives.PAS_HEALING_ITEM_VISION ), "Player has the wrong passive for this!" )

	switch( lootData.ref )
	{
		case "health_pickup_health_large":
		case "health_pickup_combo_large":
		case "health_pickup_combo_full":
			return true
	}
	return false
}

bool function ReconClass_ShouldSeeLoot( entity loot, entity player, LootData lootData )
{
	return lootData.lootType == eLootType.ATTACHMENT && lootData.attachmentStyle == "sight"
}

void function DeathBoxInsight_CreateInWorldMarker()
{
	entity localViewPlayer = GetLocalViewPlayer()
	var rui                = CreateFullscreenRui( $"ui/death_box_insight_icon.rpak", RuiCalculateDistanceSortKey( localViewPlayer.EyePosition(), <0,0,0> ) )


	if ( PlayerHasPassive( localViewPlayer, ePassives.PAS_ALTER ) )
	{
		RuiSetFloat( rui, "minAlphaDist", 2500 )
		RuiSetFloat( rui, "maxAlphaDist", 3000 )
	}
	else

	{
		RuiSetFloat( rui, "minAlphaDist", 500 )
		RuiSetFloat( rui, "maxAlphaDist", 1000 )
	}
	RuiSetFloat( rui, "upOffset", ICON_UP_OFFSET )

	UISize screenSize = GetVirtualScreenSize( GetScreenSize().width, GetScreenSize().height )
	RuiSetFloat2( rui, "actualRes", <float( screenSize.width ), float( screenSize.height ), 0> )
	RuiSetGameTime( rui, "startTime", Time() )
	RuiKeepSortKeyUpdated( rui, true, "pos" )
	file.deathboxRui = rui
}

struct DeathboxContentsInfo
{
	array<int> contents
	int count
}

DeathboxContentsInfo function GetContentsToDisplayFromDeathbox( entity box )
{
	DeathboxContentsInfo result
	if( !IsValid( box ) )
		return result

	entity localPlayer = GetLocalViewPlayer()
	
	if ( localPlayer.HasPassive( ePassives.PAS_DEATHBOX_BATTERY_COUNT ) )
	{
		array<entity> lootInBox = GetDeathBoxLootEnts( box )
		int batteryCount        = 0
		foreach ( entity ent in lootInBox )
		{
			LootData lootData = SURVIVAL_Loot_GetLootDataByIndex( ent.GetSurvivalInt() )
			if ( lootData.ref == "health_pickup_combo_large" )
			{
				batteryCount += ent.GetClipCount()
				if( result.contents.len() == 0 )
					result.contents.append( lootData.index )
			}
		}
		if ( batteryCount != 0 )
		{
			result.count = batteryCount
		}
		return result
	}


	array<entity> lootInBox = GetDeathBoxLootEnts( box )
	foreach( passive, callback in file.passiveToShouldSeeFunction )
	{
		if( !localPlayer.HasPassive( passive ) )
			continue
		foreach( entity ent in lootInBox )
		{
			LootData lootData = SURVIVAL_Loot_GetLootDataByIndex( ent.GetSurvivalInt() )
			if( !result.contents.contains( lootData.index ) && callback( ent, localPlayer, lootData ) )
			{
				result.contents.append( lootData.index )
			}
		}
	}
	return result
}

void function UpdateBoxContentsDisplay( entity box, var rui, DeathboxContentsInfo contents )
{
	entity localPlayer = GetLocalViewPlayer()

	if( localPlayer.HasPassive( ePassives.PAS_DEATHBOX_BATTERY_COUNT ) )
	{
		if( contents.count == 0 )
		{
			RuiSetInt( rui, "visibleCount", 0 )
		}
		else
		{
			Assert( contents.contents.len() > 0 )
			RuiSetInt( rui, "visibleCount", 1 )
			LootData data = SURVIVAL_Loot_GetLootDataByIndex( contents.contents[0] )
			RuiSetImage( rui, "image0", data.hudIcon )
		}
		RuiSetInt( rui, "leftCount", contents.count )
	}
	else
	{
		for( int i=0; i < contents.contents.len() && i < MAX_DEATHBOX_ICONS; i++ )
		{
			string ruiImageVal = "image" + i
			LootData data = SURVIVAL_Loot_GetLootDataByIndex( contents.contents[i] )
			RuiSetImage( rui, ruiImageVal, data.hudIcon )
		}
		RuiSetInt( rui, "visibleCount", minint( MAX_DEATHBOX_ICONS, contents.contents.len() ) )
		RuiSetInt( rui, "leftCount", 0 )
	}
	file.numSlotsInCurrentLookat = contents.contents.len()
}

float function ClampedDistScale( float near, float far, float nearVal, float farVal, float dist )
{
	float totalDist = far - near
	float distBetween = dist - near
	float percBetween = clamp( distBetween / totalDist, 0.0, 1.0 )
	float result = nearVal * ( 1.0 - percBetween ) + farVal * ( percBetween )
	return result
}

bool function DeathBoxInsight_UpdateLookatPing()
{
	if( !file.isActive )
		return false

	entity player = GetLocalViewPlayer()
	vector playerEyePos = player.EyePosition()
	vector viewVector = player.GetViewVector()
	vector rightVector = player.GetViewRight()
	array<entity> allBoxes = GetAllDeathBoxes()

	float maxVisibleDist = MAX_VISIBLE_DISTANCE

	bool hasRemoteDeathboxInteract = false
	if ( PlayerHasPassive( player, ePassives.PAS_ALTER ) )
	{
		hasRemoteDeathboxInteract = true
		maxVisibleDist = GetRemoteDeathboxInteractRangeForAlter( player )
	}

	array<entity> candidateBoxes = GetEntitiesFromArrayNearPos( allBoxes, playerEyePos, maxVisibleDist )
	ArrayRemoveInvalid( candidateBoxes )
	entity lookatEnt = null
	int lookatIndex = -1
	float pingDot = deg_cos( PING_DEGREES )
	float lookatDot = deg_cos( LOOKAT_DEGREES )
	DeathboxContentsInfo contentsToDisplay
	float bestDot
	float bestBoxDistance
	vector bestBoxOrigin

	array<entity> enemies = GetPlayerArrayEx( TEAM_ANY, player.GetTeam(), playerEyePos, maxVisibleDist )
	bool enemyInView = false
	foreach( entity enemy in enemies )
	{
		if( PlayerCanSee( player, enemy, true, LOOKAT_DEGREES ) )
		{
			enemyInView = true
			break
		}
	}

	if( !enemyInView )
	{
		
		array<entity> activeWps = Waypoints_GetActiveLootPings()
		foreach( wp in activeWps )
		{
			if( !IsValid( wp ) )
				continue
			entity pingedEnt =  Waypoint_GetItemEntForLootWaypoint( wp )
			entity pingedParent = pingedEnt.GetParent()
			if( IsValid( pingedParent ) && candidateBoxes.contains( pingedParent ) )
			{
				candidateBoxes.fastremovebyvalue( pingedParent )
			}
		}

		
		foreach( entity box in candidateBoxes )
		{
			DeathboxContentsInfo tempContents = GetContentsToDisplayFromDeathbox( box )
			if( box == file.currentLookat )
			{
				contentsToDisplay = tempContents
			}
			
			if( tempContents.contents.len() == 0 )
				continue

			vector boxOrigin = box.GetOrigin()
			vector offsetCenter = box.GetOrigin() + <0,0, ICON_UP_OFFSET>
			vector playerToOffsetCenter = Normalize( offsetCenter - playerEyePos )
			float centerDotProduct =  DotProduct( playerToOffsetCenter, viewVector )
			
			if( centerDotProduct < lookatDot || centerDotProduct < bestDot )
				continue


			bool canRemoteInteractWithBox = false
			if ( hasRemoteDeathboxInteract )
			{
				canRemoteInteractWithBox = CanPlayerRemoteInteractWithDeathbox( player, box )

				if ( !canRemoteInteractWithBox && !IsPlayerWithinStandardDeathBoxUseDistance( player, box ))
					continue
			}
			if ( !canRemoteInteractWithBox )

			{
				
				TraceResults trace = TraceLine( playerEyePos, box.GetWorldSpaceCenter(), null, TRACE_MASK_BLOCKLOS, TRACE_COLLISION_GROUP_NONE )
				if ( !( trace.hitEnt == box || trace.fraction >= 0.99 ) )
					continue
			}

			bestDot = centerDotProduct
			lookatEnt = box
			bestBoxOrigin = boxOrigin
		}
	}

	
	if( lookatEnt != file.currentLookat )
	{
		file.lookatAlpha -= FrameTime() * ALPHA_LERP_MULTIPLIER
		if ( file.lookatAlpha <= 0.0 )
		{
			file.lookatAlpha   = 0.0
			file.currentLookat = lookatEnt
			if( lookatEnt != null )
			{
				RuiTrackFloat3( file.deathboxRui, "pos", lookatEnt, RUI_TRACK_ABSORIGIN_FOLLOW )
			}
		}
	}

	if( file.currentLookat != null )
	{
		if( enemyInView )
		{
			contentsToDisplay = GetContentsToDisplayFromDeathbox( file.currentLookat )
		}

		UpdateBoxContentsDisplay( file.currentLookat, file.deathboxRui, contentsToDisplay )
	}

	
	if( file.lookatAlpha < 1.0 )
	{
		if( lookatEnt == file.currentLookat && lookatEnt != null )
		{
			file.lookatAlpha = clamp( file.lookatAlpha + FrameTime() * ALPHA_LERP_MULTIPLIER, 0.0, 1.0 )
		}
		RuiSetFloat( file.deathboxRui, "alphaTweak", file.lookatAlpha )
		RuiSetInt( file.deathboxRui, "highlightedSlot", -1 )
		file.currentLookatIndex = -1
		file.currentLookatLootIndex = -1
		return false
	}
	
	int lookatLootIndex = -1

	if( file.currentLookat != null && DistanceSqr( playerEyePos, bestBoxOrigin ) < MAX_PING_DISTANCE * MAX_PING_DISTANCE && Perks_GetPerkPingInfo().ent == null )
	{
		float dist = Distance( playerEyePos, bestBoxOrigin )
		float bestIndexDot = 0
		for( int i=0; i < file.numSlotsInCurrentLookat; i++ )
		{
			float sideOffset = 0
			if( file.numSlotsInCurrentLookat == 2 )
			{
				
				
				
				sideOffset = ClampedDistScale( 7.0, 300.0, 2.5, 10.0, dist ) * ( i == 0 ? -1.0 : 1.0 )
			}
			else if( file.numSlotsInCurrentLookat == 3 )
			{
				if( i != 1 )
				{
					sideOffset = ClampedDistScale( 7.0, 300.0, 3, 25.0, dist ) * ( i == 0 ? -1.0 : 1.0 )
				}
			}
			vector offsetOrigin = bestBoxOrigin + rightVector * sideOffset + < 0,0, ICON_UP_OFFSET >
			vector playerToOffsetOrigin = Normalize( offsetOrigin - playerEyePos )
			float dotProduct = DotProduct( playerToOffsetOrigin, viewVector )
			if( dotProduct < pingDot || dotProduct < bestIndexDot )
				continue
			if( file.numSlotsInCurrentLookat == 2 && i == 1 )
				lookatIndex = 2
			else
				lookatIndex = i
			bestIndexDot = dotProduct
			lookatLootIndex = contentsToDisplay.contents[i]
		}
	}


	if( file.currentLookatIndex != lookatIndex )
	{
		file.currentLookatIndex = lookatIndex
		file.currentLookatLootIndex = lookatLootIndex
		RuiSetInt( file.deathboxRui, "highlightedSlot", lookatIndex )
	}

	return lookatIndex >= 0
}

DeathBoxPingInfo function DeathBoxInsight_GetCurrentPingInfo()
{
	DeathBoxPingInfo result
	result.deathbox = file.currentLookat

	if( file.currentLookat != null && file.currentLookatIndex >= 0 )
	{
		array<entity> lootInBox = GetDeathBoxLootEnts( result.deathbox )
		foreach( entity ent in lootInBox )
		{
			LootData lootData = SURVIVAL_Loot_GetLootDataByIndex( ent.GetSurvivalInt() )
			if( lootData.index == file.currentLookatLootIndex )
			{
				result.loot = ent
				return result
			}
		}

	}
	return result
}



