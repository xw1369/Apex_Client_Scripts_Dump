global function OnWeaponActivate_ability_rise_from_the_ashes
global function OnWeaponTossReleaseAnimEvent_ability_rise_from_the_ashes
global function MpAbilityRiseFromTheAshes_Init

global const string REVENANT_SHADOW_RISE_FROM_ASHES_ULT_WEAPON_NAME = "mp_ability_rise_from_the_ashes"
const string SHADOW_FORM_WINDUP_SOUND_1P = "DeathProtection_Reborn_Windup_1P"
const string SHADOW_FORM_WINDUP_SOUND_3P = "DeathProtection_Reborn_Windup_3P"

void function MpAbilityRiseFromTheAshes_Init()
{
}



void function OnWeaponActivate_ability_rise_from_the_ashes( entity weapon )
{
	entity player = weapon.GetOwner()
	if( !IsValid( player ) )
		return










}

var function OnWeaponTossReleaseAnimEvent_ability_rise_from_the_ashes( entity weapon, WeaponPrimaryAttackParams attackParams )
{
	entity ownerPlayer = weapon.GetOwner()
	Assert( ownerPlayer.IsPlayer() )





	PlayerUsedOffhand( ownerPlayer, weapon )

	return weapon.GetAmmoPerShot()
}

