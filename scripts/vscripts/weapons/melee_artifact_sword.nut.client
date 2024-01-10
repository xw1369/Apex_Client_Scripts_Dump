

global function MeleeArtifactSword_Init

global function OnWeaponActivate_melee_artifact_sword
global function OnWeaponDeactivate_melee_artifact_sword


const GH_SWORD_ATTACK_FX_1P = $"P_sword_melee_swipe_1P"
const GH_SWORD_ATTACK_FX_3P = $"P_sword_melee_swipe_3P"






void function MeleeArtifactSword_Init()
{
	
	PrecacheParticleSystem( GH_SWORD_ATTACK_FX_1P )
	PrecacheParticleSystem( GH_SWORD_ATTACK_FX_3P )





}

void function OnWeaponActivate_melee_artifact_sword( entity weapon )
{
	entity player = weapon.GetWeaponOwner()
	string meleeSkinName = MeleeSkin_GetSkinNameFromPlayer( player )

	weapon.PlayWeaponEffect( GH_SWORD_ATTACK_FX_1P, GH_SWORD_ATTACK_FX_3P, "blade_base", true )

	if ( meleeSkinName == "artifact" )
	{


	}









}

void function OnWeaponDeactivate_melee_artifact_sword( entity weapon )
{
	entity player = weapon.GetWeaponOwner()
	string meleeSkinName = MeleeSkin_GetSkinNameFromPlayer( player )

	weapon.StopWeaponEffect( GH_SWORD_ATTACK_FX_1P, GH_SWORD_ATTACK_FX_3P )

	if ( meleeSkinName == "artifact" )
	{
		

	}







}

                              