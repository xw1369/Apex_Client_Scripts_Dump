
global function Candy_Init

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

bool function Candy_ItemPickup( entity pickup, entity player, int pickupFlags, entity deathBox, int ornull desiredCount, LootData data )
{
	
	int EVO_Reward = GetPlaylistVarInt( GetCurrentPlaylistName(), "candy_evo_points", CANDY_EVO_POINTS_DEFAULT )
	int SHIELD_Reward = GetPlaylistVarInt( GetCurrentPlaylistName(), "candy_shield_heal", CANDY_SHIELD_HEAL )
	int ULT_Reward = GetPlaylistVarInt( GetCurrentPlaylistName(), "candy_ult_charge", CANDY_ULT_CHARGE )
	float DURATION_Reward = GetPlaylistVarFloat( GetCurrentPlaylistName(), "candy_duration", CANDY_SHIELD_HEAL_DURATION )

	bool Candy_Give_Ultimate = GetPlaylistVarBool( GetCurrentPlaylistName(), "candy_give_ultimate", CANDY_GIVE_ULTIMATE )
















		
		if (player == GetLocalClientPlayer())
		{
			AnnouncementMessageRight( GetLocalClientPlayer(), "EVO: +" + EVO_Reward + "\nULT: +" + ULT_Reward + "%", "", <0, 1, 0>, $"", 1.0 )

			
			int fxIndex = GetParticleSystemIndex( CANDY_PICKUP_WORLD_FX )
			StartParticleEffectInWorld( fxIndex, pickup.GetOrigin(), <0, 0, 0> )
		}

		
		thread Candy_ScreenFx( player )


	return true
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












































































































                        