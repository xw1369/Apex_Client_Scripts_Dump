
global function MeleeBangaloreHeirloomRt01_Init

global function OnWeaponActivate_melee_bangalore_heirloom_rt01
global function OnWeaponDeactivate_melee_bangalore_heirloom_rt01

const GHURKA_FX_ATTACK_SWIPE_FP = $"P_wpn_ghurka_rt01_swipe_FP"
const GHURKA_FX_ATTACK_SWIPE_3P = $"P_wpn_ghurka_rt01_swipe_3P"

void function MeleeBangaloreHeirloomRt01_Init()
{
	PrecacheParticleSystem( GHURKA_FX_ATTACK_SWIPE_FP )
	PrecacheParticleSystem( GHURKA_FX_ATTACK_SWIPE_3P )

}

void function OnWeaponActivate_melee_bangalore_heirloom_rt01( entity weapon )
{
	entity player = weapon.GetWeaponOwner()
	string meleeSkinName = MeleeSkin_GetSkinNameFromPlayer( player )

	
	
		weapon.PlayWeaponEffect( GHURKA_FX_ATTACK_SWIPE_FP, GHURKA_FX_ATTACK_SWIPE_3P, "blade_front" )
	

}

void function OnWeaponDeactivate_melee_bangalore_heirloom_rt01( entity weapon )
{
	entity player = weapon.GetWeaponOwner()
	string meleeSkinName = MeleeSkin_GetSkinNameFromPlayer( player )

	
	
		weapon.StopWeaponEffect( GHURKA_FX_ATTACK_SWIPE_FP, GHURKA_FX_ATTACK_SWIPE_3P )
	

}


