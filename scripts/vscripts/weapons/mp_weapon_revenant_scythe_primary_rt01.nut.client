global function MpWeaponRevenantScythePrimary_rt01_Init

global function OnWeaponActivate_weapon_revenant_scythe_primary_rt01
global function OnWeaponDeactivate_weapon_revenant_scythe_primary_rt01

const asset SCY_FX_BLADE_PC = $"P_scy_blade_pc1"
const asset SCY_FX_BLADE_PC_2 = $"P_scy_blade_pc2"
const asset SCY_FX_BLADE_PC_3 = $"P_scy_blade_pc3"
const asset SCY_FX_BASE = $"P_scy_blade_base"

const asset SCY_FX_BLADE_PC_3P = $"P_scy_blade_pc1_3P"
const asset SCY_FX_BLADE_PC_2_3P = $"P_scy_blade_pc2_3P"
const asset SCY_FX_BASE_3P = $"P_scy_blade_base_3P"


const asset SCY_RT01_FX_BLADE_PC_1_1P = $"P_scy_rt01_blade_pc1_1p"
const asset SCY_RT01_FX_BLADE_PC_2_1P = $"P_scy_rt01_blade_pc2_1p"
const asset SCY_RT01_FX_BLADE_PC_3_1P = $"P_scy_rt01_blade_pc3_1p"
const asset SCY_RT01_FX_BASE_1P = $"P_scy_rt01_blade_base_1p"

const asset SCY_RT01_FX_BLADE_PC_1_3P = $"P_scy_rt01_blade_pc1_3p"
const asset SCY_RT01_FX_BLADE_PC_2_3P = $"P_scy_rt01_blade_pc2_3p"
const asset SCY_RT01_FX_BASE_3P = $"P_scy_rt01_blade_base_3p"

const table<string, table< int, int > > HEIRLOOM_SKIN_REMAP =
{
	["scythe_rt01"] =
	{
		[eDamageSourceId.mp_weapon_revenant_scythe] = eDamageSourceId.mp_weapon_revenant_scythe_rt01,
		[eDamageSourceId.melee_revenant_scythe] = eDamageSourceId.melee_revenant_scythe_rt01,
	},
}


void function MpWeaponRevenantScythePrimary_rt01_Init()
{
	PrecacheParticleSystem( SCY_FX_BLADE_PC )
	PrecacheParticleSystem( SCY_FX_BLADE_PC_2 )
	PrecacheParticleSystem( SCY_FX_BLADE_PC_3 )
	PrecacheParticleSystem( SCY_FX_BASE )

	PrecacheParticleSystem( SCY_FX_BLADE_PC_3P )
	PrecacheParticleSystem( SCY_FX_BLADE_PC_2_3P )
	PrecacheParticleSystem( SCY_FX_BASE_3P )


	PrecacheParticleSystem( SCY_RT01_FX_BLADE_PC_1_1P )
	PrecacheParticleSystem( SCY_RT01_FX_BLADE_PC_2_1P )
	PrecacheParticleSystem( SCY_RT01_FX_BLADE_PC_3_1P )
	PrecacheParticleSystem( SCY_RT01_FX_BASE_1P )

	PrecacheParticleSystem( SCY_RT01_FX_BLADE_PC_1_3P )
	PrecacheParticleSystem( SCY_RT01_FX_BLADE_PC_2_3P )
	PrecacheParticleSystem( SCY_RT01_FX_BASE_3P )





}












void function OnWeaponActivate_weapon_revenant_scythe_primary_rt01( entity weapon )
{
	entity player = weapon.GetWeaponOwner()
	string meleeSkinName = MeleeSkin_GetSkinNameFromPlayer( player )
	if ( meleeSkinName == "scythe" )
	{
		weapon.PlayWeaponEffect( SCY_FX_BLADE_PC_3, SCY_FX_BLADE_PC_2_3P, "blade_piece_fx_01", true )
		weapon.PlayWeaponEffect( SCY_FX_BLADE_PC, SCY_FX_BLADE_PC_3P, "blade_piece_fx_02", true )
		weapon.PlayWeaponEffect( SCY_FX_BLADE_PC_2, SCY_FX_BLADE_PC_2_3P, "blade_piece_fx_03", true )
		weapon.PlayWeaponEffect( SCY_FX_BLADE_PC, SCY_FX_BLADE_PC_3P, "blade_piece_fx_04", true )
		weapon.PlayWeaponEffect( SCY_FX_BLADE_PC_3, SCY_FX_BLADE_PC_2_3P, "blade_piece_fx_05", true )
		weapon.PlayWeaponEffect( SCY_FX_BLADE_PC, SCY_FX_BLADE_PC_3P, "blade_piece_fx_06", true )
		weapon.PlayWeaponEffect( SCY_FX_BLADE_PC_2, SCY_FX_BLADE_PC_2_3P, "blade_piece_fx_07", true )
		weapon.PlayWeaponEffect( SCY_FX_BLADE_PC, SCY_FX_BLADE_PC_3P, "blade_piece_fx_08", true )
		weapon.PlayWeaponEffect( SCY_FX_BASE, SCY_FX_BASE_3P, "blade_base_fx", true )
	}
	else if ( meleeSkinName == "scythe_rt01" )
	{
		weapon.PlayWeaponEffect( SCY_RT01_FX_BLADE_PC_3_1P, SCY_RT01_FX_BLADE_PC_2_3P, "blade_piece_fx_01", true )
		weapon.PlayWeaponEffect( SCY_RT01_FX_BLADE_PC_1_1P, SCY_RT01_FX_BLADE_PC_1_3P, "blade_piece_fx_02", true )
		weapon.PlayWeaponEffect( SCY_RT01_FX_BLADE_PC_2_1P, SCY_RT01_FX_BLADE_PC_2_3P, "blade_piece_fx_03", true )
		weapon.PlayWeaponEffect( SCY_RT01_FX_BLADE_PC_1_1P, SCY_RT01_FX_BLADE_PC_1_3P, "blade_piece_fx_04", true )
		weapon.PlayWeaponEffect( SCY_RT01_FX_BLADE_PC_3_1P, SCY_RT01_FX_BLADE_PC_2_3P, "blade_piece_fx_05", true )
		weapon.PlayWeaponEffect( SCY_RT01_FX_BLADE_PC_1_1P, SCY_RT01_FX_BLADE_PC_1_3P, "blade_piece_fx_06", true )
		weapon.PlayWeaponEffect( SCY_RT01_FX_BLADE_PC_2_1P, SCY_RT01_FX_BLADE_PC_2_3P, "blade_piece_fx_07", true )
		weapon.PlayWeaponEffect( SCY_RT01_FX_BLADE_PC_1_1P, SCY_RT01_FX_BLADE_PC_1_3P, "blade_piece_fx_08", true )
		weapon.PlayWeaponEffect( SCY_RT01_FX_BASE_1P, SCY_RT01_FX_BASE_3P, "blade_base_fx", true )
	}

}

void function OnWeaponDeactivate_weapon_revenant_scythe_primary_rt01( entity weapon )
{
	entity player = weapon.GetWeaponOwner()
	string meleeSkinName = MeleeSkin_GetSkinNameFromPlayer( player )
	if ( meleeSkinName == "scythe" )
	{
		weapon.StopWeaponEffect( SCY_FX_BLADE_PC, SCY_FX_BLADE_PC_3P )
		weapon.StopWeaponEffect( SCY_FX_BLADE_PC_2, SCY_FX_BLADE_PC_2_3P )
		weapon.StopWeaponEffect( SCY_FX_BLADE_PC_3, SCY_FX_BLADE_PC_2_3P )
		weapon.StopWeaponEffect( SCY_FX_BASE, SCY_FX_BASE_3P )
	}
	else if ( meleeSkinName == "scythe_rt01" )
	 {
		weapon.StopWeaponEffect( SCY_RT01_FX_BLADE_PC_1_1P, SCY_RT01_FX_BLADE_PC_1_3P )
		weapon.StopWeaponEffect( SCY_RT01_FX_BLADE_PC_2_1P, SCY_RT01_FX_BLADE_PC_2_3P )
		weapon.StopWeaponEffect( SCY_RT01_FX_BLADE_PC_3_1P, SCY_RT01_FX_BLADE_PC_2_3P )
		weapon.StopWeaponEffect( SCY_RT01_FX_BASE_1P, SCY_RT01_FX_BASE_3P )
	 }
}