
global function Candy_Init
global function Candy_RegisterNetworking


global function Candy_PickupAnnoucement


const int	CANDY_EVO_POINTS_DEFAULT = 75
const int   CANDY_SHIELD_HEAL = 25
const float	CANDY_SHIELD_HEAL_DURATION = 4.0
const float	CANDY_ULT_CHARGE = 20

const bool CANDY_GIVE_SPEED = false
const bool CANDY_GIVE_ULTIMATE = true

const asset CANDY_SHIELD_CHARGE_FX = $"P_armor_3P_loop_CP"

const asset CANDY_PICKUP_SCREEN_FX = $"P_candy_boost_1p"
const float CANDY_PICKUP_SCREEN_FX_DURATION = 0.5

const asset CANDY_PICKUP_WORLD_FX = $"P_candy_pickup_3p"

const string CANDY_PICKUP_SHIELD_CHARGE_SFX_1P = "Survival_Loot_Pickup_Candy_Charging_1P"
const string CANDY_PICKUP_SHIELD_CHARGE_SFX_3P = "Survival_Loot_Pickup_Candy_Charging_3P"
const string CANDY_PICKUP_SHIELD_CHARGE_END_SFX_1P = "Survival_Loot_Pickup_Candy_Charging_Ending_1P"
const string CANDY_PICKUP_SHIELD_CHARGE_END_SFX_3P = "Survival_Loot_Pickup_Candy_Charging_Ending_3P"

const int   CANDY_SHIELD_CHARGE_END_TRIGGER_AMOUNT = 3

void function Candy_Init()
{
	PrecacheParticleSystem( CANDY_SHIELD_CHARGE_FX )
	PrecacheParticleSystem( CANDY_PICKUP_SCREEN_FX )
	PrecacheParticleSystem( CANDY_PICKUP_WORLD_FX )

	RegisterCustomItemPickupAction( "candy_pickup", Candy_ItemPickup )

	RegisterSignal( "OnShieldDamaged" )



}

void function Candy_RegisterNetworking()
{
	Remote_RegisterClientFunction( "Candy_PickupAnnoucement", "entity", "int", INT_MIN, INT_MAX, "int", INT_MIN, INT_MAX, "vector", -FLT_MAX, FLT_MAX, 32 )
}

bool function Candy_ItemPickup( entity pickup, entity player, int pickupFlags, entity deathBox, int ornull desiredCount, LootData data )
{
	
	int EVO_Reward = GetPlaylistVarInt( GetCurrentPlaylistName(), "candy_evo_points", CANDY_EVO_POINTS_DEFAULT )
	int SHIELD_Reward = GetPlaylistVarInt( GetCurrentPlaylistName(), "candy_shield_heal", CANDY_SHIELD_HEAL )
	int ULT_Reward = GetPlaylistVarInt( GetCurrentPlaylistName(), "candy_ult_charge", CANDY_ULT_CHARGE )
	float DURATION_Reward = GetPlaylistVarFloat( GetCurrentPlaylistName(), "candy_duration", CANDY_SHIELD_HEAL_DURATION )

	bool Candy_Give_Ultimate = GetPlaylistVarBool( GetCurrentPlaylistName(), "candy_give_ultimate", CANDY_GIVE_ULTIMATE )


















	return true
}

void function Candy_PickupAnnoucement( entity player, int EVO_Reward, int ULT_Reward, vector pickupLocation )
{
	
	if (player == GetLocalClientPlayer())
	{
		string messageText = ( EVO_Reward > 0 ) ? "EVO: +" + EVO_Reward + "\n" : ""
		messageText += ( ULT_Reward > 0 ) ? "ULT: +" + ULT_Reward + "%" : ""
		if ( messageText.len() > 0 )
			AnnouncementMessageRight( GetLocalClientPlayer(), messageText, "", <0, 1, 0>, $"", 1.0 )

		
		int fxIndex = GetParticleSystemIndex( CANDY_PICKUP_WORLD_FX )
		StartParticleEffectInWorld( fxIndex, pickupLocation, <0, 0, 0> )
	}

	
	thread Candy_ScreenFx( player )
}



void function Candy_ScreenFx ( entity player )
{
	EndSignal( player, "OnDeath", "OnDestroy" )

	int candyScreenFxHandle

	if ( player != GetLocalViewPlayer() )
		return

	entity cockpit = player.GetCockpit()
	if ( !IsValid( cockpit ) )
		return

	if ( EffectDoesExist( candyScreenFxHandle ) )
		return

	int fxID = GetParticleSystemIndex( CANDY_PICKUP_SCREEN_FX )
	candyScreenFxHandle = StartParticleEffectOnEntity( cockpit, fxID, FX_PATTACH_ABSORIGIN_FOLLOW, ATTACHMENTID_INVALID )
	EffectSetIsWithCockpit( candyScreenFxHandle, true )
	EffectSetControlPointVector( candyScreenFxHandle, 1, <255, 208, 56> )

	OnThreadEnd( function() : ( candyScreenFxHandle )
	{
		if ( EffectDoesExist( candyScreenFxHandle ) )
			EffectStop( candyScreenFxHandle, false, true )
	} )

	wait CANDY_PICKUP_SCREEN_FX_DURATION
}












































































































                        