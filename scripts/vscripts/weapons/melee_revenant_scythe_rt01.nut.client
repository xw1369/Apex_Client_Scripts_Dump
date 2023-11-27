
global function MeleeRevenantScythe_rt01_Init

global function OnWeaponActivate_melee_revenant_scythe_rt01
global function OnWeaponDeactivate_melee_revenant_scythe_rt01

const REV_SCY_FX_ATTACK_SWIPE_FP = $"P_scy_blade_swipe_FP"
const REV_SCY_FX_ATTACK_SWIPE_3P = $"P_scy_blade_swipe_3P"


const REV_SCY_RT01_FX_ATTACK_SWIPE_1P = $"P_scy_rt01_blade_swipe_1p"
const REV_SCY_RT01_FX_ATTACK_SWIPE_3P = $"P_scy_rt01_blade_swipe_3p"


void function MeleeRevenantScythe_rt01_Init()
{

	PrecacheParticleSystem( REV_SCY_FX_ATTACK_SWIPE_FP )
	PrecacheParticleSystem( REV_SCY_FX_ATTACK_SWIPE_3P )

	PrecacheParticleSystem( REV_SCY_RT01_FX_ATTACK_SWIPE_1P )
	PrecacheParticleSystem( REV_SCY_RT01_FX_ATTACK_SWIPE_3P )

}



void function OnWeaponActivate_melee_revenant_scythe_rt01( entity weapon )
{
	entity player = weapon.GetWeaponOwner()
	string meleeSkinName = MeleeSkin_GetSkinNameFromPlayer( player )

	if ( meleeSkinName == "scythe" )
	{
		weapon.PlayWeaponEffect( REV_SCY_FX_ATTACK_SWIPE_FP, REV_SCY_FX_ATTACK_SWIPE_3P, "blade_base_fx" )
	}
	else if ( meleeSkinName == "scythe_rt01" )
	{
		weapon.PlayWeaponEffect( REV_SCY_RT01_FX_ATTACK_SWIPE_1P, REV_SCY_RT01_FX_ATTACK_SWIPE_3P, "blade_base_fx" )
	}


}

void function OnWeaponDeactivate_melee_revenant_scythe_rt01( entity weapon )
{
	entity player = weapon.GetWeaponOwner()
	string meleeSkinName = MeleeSkin_GetSkinNameFromPlayer( player )

	if ( meleeSkinName == "scythe" )
	{
		weapon.StopWeaponEffect( REV_SCY_FX_ATTACK_SWIPE_FP, REV_SCY_FX_ATTACK_SWIPE_3P )
	}
	else if ( meleeSkinName == "scythe_rt01" )
	{
		weapon.StopWeaponEffect( REV_SCY_RT01_FX_ATTACK_SWIPE_1P, REV_SCY_RT01_FX_ATTACK_SWIPE_3P )
	}

}
      