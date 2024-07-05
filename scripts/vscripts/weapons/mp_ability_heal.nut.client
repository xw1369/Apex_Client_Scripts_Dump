global function OnWeaponChargeBegin_ability_heal
global function OnWeaponChargeEnd_ability_heal
global function OnWeaponAttemptOffhandSwitch_ability_heal

const int STIM_HEALTH_COST = 20
const int STIM_HEALTH_COST_UPGRADE = 5

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

bool function OnWeaponChargeBegin_ability_heal( entity weapon )
{
	entity ownerPlayer = weapon.GetWeaponOwner()
	float duration     = weapon.GetWeaponSettingFloat( eWeaponVar.charge_time )
	StimPlayerWithOffhandWeapon( ownerPlayer, duration, weapon )
























	PlayerUsedOffhand( ownerPlayer, weapon )








		Rumble_Play( "rumble_stim_activate", {} )

	return true
}


void function OnWeaponChargeEnd_ability_heal( entity weapon )
{




}


bool function OnWeaponAttemptOffhandSwitch_ability_heal( entity weapon )
{
	return true
}