

global function MpWeaponFuseHeirloomPrimary_Init
global function OnWeaponActivate_weapon_fuse_heirloom_primary
global function OnWeaponDeactivate_weapon_fuse_heirloom_primary


const asset GUITAR_FX_BASE_FP = $"P_fuse_guitar_base" 
const asset GUITAR_FX_BASE_3P = $"P_fuse_guitar_base_3P"





void function MpWeaponFuseHeirloomPrimary_Init()
{
	
	PrecacheParticleSystem( GUITAR_FX_BASE_FP )
	PrecacheParticleSystem( GUITAR_FX_BASE_3P )






}

void function OnWeaponActivate_weapon_fuse_heirloom_primary( entity weapon )
{
	entity player = weapon.GetWeaponOwner()
	string meleeSkinName = MeleeSkin_GetSkinNameFromPlayer( player )

	if ( meleeSkinName == "heirloom" )
	{
		
		weapon.PlayWeaponEffect( GUITAR_FX_BASE_FP, GUITAR_FX_BASE_3P, "fx_blade_btm", true )

	}







}

void function OnWeaponDeactivate_weapon_fuse_heirloom_primary( entity weapon )
{
	entity player = weapon.GetWeaponOwner()
	string meleeSkinName = MeleeSkin_GetSkinNameFromPlayer( player )

	if ( meleeSkinName == "heirloom" )
	{
		
		weapon.StopWeaponEffect( GUITAR_FX_BASE_FP, GUITAR_FX_BASE_3P )

	}







}
                         