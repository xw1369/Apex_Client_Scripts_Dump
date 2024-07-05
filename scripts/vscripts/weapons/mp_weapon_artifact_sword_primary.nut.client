

global function MpWeaponArtifactSwordPrimary_Init
global function OnWeaponActivate_weapon_artifact_sword_primary
global function OnWeaponDeactivate_weapon_artifact_sword_primary

const GH_SWORD_IDLE_FX_1P = $"P_sword_melee_idle_1P"
const GH_SWORD_IDLE_FX_3P = $"P_sword_melee_idle_3P"





void function MpWeaponArtifactSwordPrimary_Init()
{
	
	PrecacheParticleSystem( GH_SWORD_IDLE_FX_1P )
	PrecacheParticleSystem( GH_SWORD_IDLE_FX_3P )



}

void function OnWeaponActivate_weapon_artifact_sword_primary( entity weapon )
{
	entity player = weapon.GetWeaponOwner()
	string meleeSkinName = MeleeSkin_GetSkinNameFromPlayer( player )

	weapon.PlayWeaponEffect( GH_SWORD_IDLE_FX_1P, GH_SWORD_IDLE_FX_3P, "blade_base" )

	Melee_SetModsForLegendAbilities( player )

	if ( meleeSkinName == "heirloom" )
	{
		
	}






}

void function OnWeaponDeactivate_weapon_artifact_sword_primary( entity weapon )
{
	entity player = weapon.GetWeaponOwner()
	string meleeSkinName = MeleeSkin_GetSkinNameFromPlayer( player )

	weapon.StopWeaponEffect( GH_SWORD_IDLE_FX_1P, GH_SWORD_IDLE_FX_3P )

	Melee_RemoveModsForLegendAbilities( player )

	if ( meleeSkinName == "heirloom" )
	{
		
	}






}

                              