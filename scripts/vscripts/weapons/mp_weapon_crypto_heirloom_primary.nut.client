global function MpWeaponCryptoHeirloomPrimary_Init
global function OnWeaponActivate_weapon_crypto_heirloom_primary
global function OnWeaponDeactivate_weapon_crypto_heirloom_primary


const asset CRYPTO_AMB_EXHAUST_FP = $"P_crypto_sword_exhaust"
const asset CRYPTO_AMB_EXHAUST_3P = $"P_crypto_sword_base_3P"  


const string SWORD_RT01_MOD = "sword_rt01"


const table<string, table< int, int > > HEIRLOOM_SKIN_REMAP =
{
	["heirloom_rt01"] =
	{
		[eDamageSourceId.mp_weapon_crypto_heirloom] = eDamageSourceId.mp_weapon_crypto_heirloom_rt01_primary,
		[eDamageSourceId.melee_crypto_heirloom] = eDamageSourceId.melee_crypto_heirloom_rt01,
	},
}

void function MpWeaponCryptoHeirloomPrimary_Init()
{

	PrecacheParticleSystem( CRYPTO_AMB_EXHAUST_FP )
	PrecacheParticleSystem( CRYPTO_AMB_EXHAUST_3P )









}








void function OnWeaponActivate_weapon_crypto_heirloom_primary( entity weapon )
{
	entity player = weapon.GetWeaponOwner()
	string meleeSkinName = MeleeSkin_GetSkinNameFromPlayer( player )

	if ( meleeSkinName == "heirloom" )
	{
		weapon.PlayWeaponEffect( CRYPTO_AMB_EXHAUST_FP, CRYPTO_AMB_EXHAUST_3P, "Fx_def_blade_01", true )
		weapon.PlayWeaponEffect( CRYPTO_AMB_EXHAUST_FP, CRYPTO_AMB_EXHAUST_3P, "Fx_def_blade_02", true )
		weapon.PlayWeaponEffect( CRYPTO_AMB_EXHAUST_FP, CRYPTO_AMB_EXHAUST_3P, "Fx_def_blade_03", true )

		if ( IsServer() || InPrediction() )
			weapon.RemoveMod( SWORD_RT01_MOD )
	}

	else if ( meleeSkinName == "heirloom_rt01" )
	{
		if ( IsServer() || InPrediction() )
			weapon.AddMod( SWORD_RT01_MOD )
	}


}

void function OnWeaponDeactivate_weapon_crypto_heirloom_primary( entity weapon )
{
	entity player = weapon.GetWeaponOwner()
	string meleeSkinName = MeleeSkin_GetSkinNameFromPlayer( player )

	if ( meleeSkinName == "heirloom" )
	{
		weapon.StopWeaponEffect( CRYPTO_AMB_EXHAUST_FP, CRYPTO_AMB_EXHAUST_3P )
	}







}