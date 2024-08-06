global function OnWeaponActivate_ability_octane_stim
global function OnWeaponChargeBegin_ability_octane_stim
global function OnWeaponChargeEnd_ability_octane_stim
global function OnWeaponAttemptOffhandSwitch_ability_octane_stim

const float 	STIM_DURATION = 6.0
const int 	STIM_HEALTH_COST = 20
const int 	STIM_HEALTH_COST_UPGRADE = 5


int function GetOctaneHealthCost( entity weapon )
{
	int result = GetCurrentPlaylistVarInt( "octane_health_cost", STIM_HEALTH_COST )
	if( !IsValid( weapon ) )
		return result
	if( !IsValid( weapon.GetOwner() ) )
		return result

	if( weapon.GetOwner().HasPassive( ePassives.PAS_TAC_UPGRADE_ONE ) ) 
		result -= STIM_HEALTH_COST_UPGRADE
	if( weapon.GetOwner().HasPassive( ePassives.PAS_TAC_UPGRADE_TWO ) ) 
		result -= STIM_HEALTH_COST_UPGRADE


	return result
}

float function GetStimDuration( )
{
	float duration = GetCurrentPlaylistVarFloat( "octane_stim_duration", STIM_DURATION )
	return duration
}

void function OnWeaponActivate_ability_octane_stim( entity weapon )
{

	const string AKIMBO_ACTIVE_MOD = "akimbo_active"
	entity owner = weapon.GetWeaponOwner()
	if ( !IsValid( owner ) )
		return


	if ( InPrediction() )

	{
		if ( owner.GetAkimboState() == AKIMBO_STATE_ACTIVE || owner.GetAkimboState() == AKIMBO_STATE_OFFHAND)
		{
			weapon.AddMod( AKIMBO_ACTIVE_MOD )

				if ( owner == GetLocalViewPlayer() )
					EmitSoundOnEntity( owner,  weapon.GetWeaponSettingString( eWeaponVar.charge_sound_1p ) )

		}
		else
		{
			weapon.RemoveMod( AKIMBO_ACTIVE_MOD )
		}
	}

}

bool function OnWeaponChargeBegin_ability_octane_stim( entity weapon )
{
	entity ownerPlayer = weapon.GetWeaponOwner()
	float duration     = GetStimDuration()
	StimPlayerWithOffhandWeapon( ownerPlayer, duration, weapon )
























	PlayerUsedOffhand( ownerPlayer, weapon )








		Rumble_Play( "rumble_stim_activate", {} )

	return true
}


void function OnWeaponChargeEnd_ability_octane_stim( entity weapon )
{




}


bool function OnWeaponAttemptOffhandSwitch_ability_octane_stim( entity weapon )
{
	entity ownerPlayer = weapon.GetWeaponOwner()
	if ( IsValid( ownerPlayer ) )
	{
		if ( StatusEffect_HasSeverity( ownerPlayer, eStatusEffect.stim_visual_effect ) )
			return false
	}

	return true
}