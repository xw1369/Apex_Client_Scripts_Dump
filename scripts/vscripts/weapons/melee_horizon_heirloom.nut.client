

global function MeleeHorizonHeirloom_Init

global function OnWeaponActivate_melee_horizon_heirloom
global function OnWeaponDeactivate_melee_horizon_heirloom

const HORIZON_WEAPON_ATTACK_FX_1P = $"P_wpn_morning_star_attack"
const HORIZON_WEAPON_ATTACK_FX_3P = $"P_wpn_morning_star_attack_3p"
const HORIZON_WEAPON_ATTACK_LIGHT_FX_1P = $"P_wpn_morning_star_light"
const HORIZON_WEAPON_ATTACK_LIGHT_FX_3P = $"P_wpn_morning_star_light_3p"





void function MeleeHorizonHeirloom_Init()
{
	PrecacheParticleSystem( HORIZON_WEAPON_ATTACK_FX_1P )
	PrecacheParticleSystem( HORIZON_WEAPON_ATTACK_FX_3P )
	PrecacheParticleSystem( HORIZON_WEAPON_ATTACK_LIGHT_FX_1P )
	PrecacheParticleSystem( HORIZON_WEAPON_ATTACK_LIGHT_FX_3P )





}

void function OnWeaponActivate_melee_horizon_heirloom( entity weapon )
{
	entity player = weapon.GetWeaponOwner()
	string meleeSkinName = MeleeSkin_GetSkinNameFromPlayer( player )

	if ( meleeSkinName == "heirloom" )
	{
		weapon.PlayWeaponEffect( HORIZON_WEAPON_ATTACK_FX_1P, HORIZON_WEAPON_ATTACK_FX_3P, "FX_CORE", true )
		weapon.PlayWeaponEffect( HORIZON_WEAPON_ATTACK_LIGHT_FX_1P, HORIZON_WEAPON_ATTACK_LIGHT_FX_3P, "FX_CORE", true )
	}







}

void function OnWeaponDeactivate_melee_horizon_heirloom( entity weapon )
{
	entity player = weapon.GetWeaponOwner()
	string meleeSkinName = MeleeSkin_GetSkinNameFromPlayer( player )

	if ( meleeSkinName == "heirloom" )
	{
		weapon.StopWeaponEffect( HORIZON_WEAPON_ATTACK_FX_1P, HORIZON_WEAPON_ATTACK_FX_3P )
		weapon.StopWeaponEffect( HORIZON_WEAPON_ATTACK_LIGHT_FX_1P, HORIZON_WEAPON_ATTACK_LIGHT_FX_3P )
	}







}

                         