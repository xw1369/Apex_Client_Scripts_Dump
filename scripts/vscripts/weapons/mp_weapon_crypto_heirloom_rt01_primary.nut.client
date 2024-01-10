
global function MpWeaponCryptoHeirloomRt01Primary_Init
global function OnWeaponActivate_weapon_crypto_heirloom_rt01_primary
global function OnWeaponDeactivate_weapon_crypto_heirloom_rt01_primary


const asset CRYPTO_AMB_EXHAUST_FP_rt01 = $"P_crypto_sword_rt01_exhaust"
const asset CRYPTO_AMB_EXHAUST_3P_rt01 = $"P_crypto_sword_rt01_base_3P"  

const table<string, table< int, int > > HEIRLOOM_SKIN_REMAP =
{
	["heirloom_rt01"] =
	{
		[eDamageSourceId.mp_weapon_crypto_heirloom] = eDamageSourceId.mp_weapon_crypto_heirloom_rt01_primary,
		[eDamageSourceId.melee_crypto_heirloom] = eDamageSourceId.melee_crypto_heirloom_rt01,
	},
}

		


void function MpWeaponCryptoHeirloomRt01Primary_Init()
{

	PrecacheParticleSystem( CRYPTO_AMB_EXHAUST_FP_rt01 )
	PrecacheParticleSystem( CRYPTO_AMB_EXHAUST_3P_rt01 )






		


}








void function OnWeaponActivate_weapon_crypto_heirloom_rt01_primary( entity weapon )
{
	entity player = weapon.GetWeaponOwner()
	string meleeSkinName = MeleeSkin_GetSkinNameFromPlayer( player )

	if ( meleeSkinName == "heirloom" )
	{
		weapon.PlayWeaponEffect( CRYPTO_AMB_EXHAUST_FP_rt01, CRYPTO_AMB_EXHAUST_3P_rt01, "Fx_def_blade_01", true )
		weapon.PlayWeaponEffect( CRYPTO_AMB_EXHAUST_FP_rt01, CRYPTO_AMB_EXHAUST_3P_rt01, "Fx_def_blade_02", true )
		weapon.PlayWeaponEffect( CRYPTO_AMB_EXHAUST_FP_rt01, CRYPTO_AMB_EXHAUST_3P_rt01, "Fx_def_blade_03", true )
	}

	else if ( meleeSkinName == "heirloom_rt01" )
	{
		
	}


}

void function OnWeaponDeactivate_weapon_crypto_heirloom_rt01_primary( entity weapon )
{
	entity player = weapon.GetWeaponOwner()
	string meleeSkinName = MeleeSkin_GetSkinNameFromPlayer( player )

	if ( meleeSkinName == "heirloom" )
	{
		weapon.StopWeaponEffect( CRYPTO_AMB_EXHAUST_FP_rt01, CRYPTO_AMB_EXHAUST_3P_rt01 )
	}

	else if ( meleeSkinName == "heirloom_rt01" )
	{
		
	}


}
                              