global function MeleeRevenantScythe_Init

global function OnWeaponActivate_melee_revenant_scythe
global function OnWeaponDeactivate_melee_revenant_scythe

const REV_SCY_FX_ATTACK_SWIPE_FP = $"P_scy_blade_swipe_FP"
const REV_SCY_FX_ATTACK_SWIPE_3P = $"P_scy_blade_swipe_3P"

const string ANIM_MELEE_SPRINT_SPIN = $"animseq/weapons/revenant_heirloom/ptpov_revenant_heirloom/melee_sprint_spin.rseq"
const string ANIM_MELEE_SLIDE = $"animseq/weapons/revenant_heirloom/ptpov_revenant_heirloom/melee_sliding.rseq"
const string ANIM_MELEE_JUMP = $"animseq/weapons/revenant_heirloom/ptpov_revenant_heirloom/melee_jump.rseq"

const REV_SCY_RT01_FX_ATTACK_SWIPE_1P = $"P_scy_rt01_blade_swipe_1p"
const REV_SCY_RT01_FX_ATTACK_SWIPE_3P = $"P_scy_rt01_blade_swipe_3p"

const REV_SCY_RT01_FX_ATTACK_SWIPE_SPRINT_SPIN_1P = $"P_scy_rt01_blade_swipe_1p"
const REV_SCY_RT01_FX_ATTACK_SWIPE_SPRINT_SPIN_3P = $"P_scy_rt01_blade_swipe_3p"

const REV_SCY_RT01_FX_ATTACK_SWIPE_SLIDE_1P = $"P_scy_rt01_blade_swipe_1p"
const REV_SCY_RT01_FX_ATTACK_SWIPE_SLIDE_3P = $"P_scy_rt01_blade_swipe_3p"

const REV_SCY_RT01_FX_ATTACK_SWIPE_JUMP_1P = $"P_scy_rt01_blade_swipe_1p"
const REV_SCY_RT01_FX_ATTACK_SWIPE_JUMP_3P = $"P_scy_rt01_blade_swipe_3p"

void function MeleeRevenantScythe_Init()
{
	PrecacheParticleSystem( REV_SCY_FX_ATTACK_SWIPE_FP )
	PrecacheParticleSystem( REV_SCY_FX_ATTACK_SWIPE_3P )

	PrecacheParticleSystem( REV_SCY_RT01_FX_ATTACK_SWIPE_1P )
	PrecacheParticleSystem( REV_SCY_RT01_FX_ATTACK_SWIPE_3P )
}



void function OnWeaponActivate_melee_revenant_scythe( entity weapon )
{
	entity player = weapon.GetWeaponOwner()
	string meleeSkinName = MeleeSkin_GetSkinNameFromPlayer( player )
	string seqName = weapon.GetCurrentSequenceName()

	if ( meleeSkinName == "scythe" )
	{
		weapon.PlayWeaponEffect( REV_SCY_FX_ATTACK_SWIPE_FP, REV_SCY_FX_ATTACK_SWIPE_3P, "blade_base_fx" )
	}
	else if ( meleeSkinName == "scythe_rt01" )
	{
		switch( seqName )
		{
			case ANIM_MELEE_SPRINT_SPIN:
				weapon.PlayWeaponEffect( REV_SCY_RT01_FX_ATTACK_SWIPE_SPRINT_SPIN_1P, REV_SCY_RT01_FX_ATTACK_SWIPE_SPRINT_SPIN_3P, "blade_base_fx" )
				break;

			case ANIM_MELEE_SLIDE:
				weapon.PlayWeaponEffect( REV_SCY_RT01_FX_ATTACK_SWIPE_SLIDE_1P, REV_SCY_RT01_FX_ATTACK_SWIPE_SLIDE_3P, "blade_base_fx" )
				break;

			case ANIM_MELEE_JUMP:
				weapon.PlayWeaponEffect( REV_SCY_RT01_FX_ATTACK_SWIPE_JUMP_1P, REV_SCY_RT01_FX_ATTACK_SWIPE_JUMP_3P, "blade_base_fx" )
				break;

			default:
				weapon.PlayWeaponEffect( REV_SCY_RT01_FX_ATTACK_SWIPE_1P, REV_SCY_RT01_FX_ATTACK_SWIPE_3P, "blade_base_fx" )
		}
	}

}

void function OnWeaponDeactivate_melee_revenant_scythe( entity weapon )
{
	entity player = weapon.GetWeaponOwner()
	string meleeSkinName = MeleeSkin_GetSkinNameFromPlayer( player )
	string seqName = weapon.GetCurrentSequenceName()

	if ( meleeSkinName == "scythe" )
	{
		weapon.StopWeaponEffect( REV_SCY_FX_ATTACK_SWIPE_FP, REV_SCY_FX_ATTACK_SWIPE_3P )
	}
	else if ( meleeSkinName == "scythe_rt01" )
	{
		switch( seqName )
		{
			case ANIM_MELEE_SPRINT_SPIN:
				weapon.StopWeaponEffect( REV_SCY_RT01_FX_ATTACK_SWIPE_SPRINT_SPIN_1P, REV_SCY_RT01_FX_ATTACK_SWIPE_SPRINT_SPIN_3P )
				break;

			case ANIM_MELEE_SLIDE:
				weapon.StopWeaponEffect( REV_SCY_RT01_FX_ATTACK_SWIPE_SLIDE_1P, REV_SCY_RT01_FX_ATTACK_SWIPE_SLIDE_3P )
				break;

			case ANIM_MELEE_JUMP:
				weapon.StopWeaponEffect( REV_SCY_RT01_FX_ATTACK_SWIPE_JUMP_1P, REV_SCY_RT01_FX_ATTACK_SWIPE_JUMP_3P )
				break;

			default:
				weapon.StopWeaponEffect( REV_SCY_RT01_FX_ATTACK_SWIPE_1P, REV_SCY_RT01_FX_ATTACK_SWIPE_3P )
		}
	}
}