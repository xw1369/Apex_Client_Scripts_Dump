
global function MeleeArtifactDagger_Init

global function OnWeaponActivate_melee_artifact_dagger
global function OnWeaponDeactivate_melee_artifact_dagger
global function OnWeaponOwnerChanged_melee_artifact_dagger

void function MeleeArtifactDagger_Init()
{
	
}

void function OnWeaponActivate_melee_artifact_dagger( entity weapon )
{
	entity player            = weapon.GetWeaponOwner()
	ArtifactFX1PAnd3P fx1P3P = Artifacts_FX_Get1PAnd3PFXArraysFromPlayerLoadouts( player, eArtifactFXPackageType.ATTACK )
	Assert( fx1P3P.fx1P.len() == fx1P3P.fx3P.len() )
	for ( int i = 0; i < fx1P3P.fx1P.len(); i++ )
		weapon.PlayWeaponEffect( fx1P3P.fx1P[i].fxAsset, fx1P3P.fx3P[i].fxAsset, fx1P3P.fx1P[i].attachName, false )
}

void function OnWeaponDeactivate_melee_artifact_dagger( entity weapon )
{
	entity player = weapon.GetWeaponOwner()

	ArtifactFX1PAnd3P fx1P3P = Artifacts_FX_Get1PAnd3PFXArraysFromPlayerLoadouts( player, eArtifactFXPackageType.ATTACK )
	for ( int i = 0; i < fx1P3P.fx1P.len(); i++ )
		weapon.StopWeaponEffect( fx1P3P.fx1P[i].fxAsset, fx1P3P.fx3P[i].fxAsset )
}

void function OnWeaponOwnerChanged_melee_artifact_dagger( entity weapon, WeaponOwnerChangedParams params )
{
	entity owner = params.newOwner

	if( !IsValid( owner ) )
		return


		if ( owner != GetLocalClientPlayer() )
			return 


	if ( !owner.IsBot() )
	{
		Artifacts_StoreLoadoutDataOnPlayerEntityStruct( owner )
		Artifacts_Loadouts_SetupWeaponComponents( weapon, owner )
		Artifacts_OnWeaponOwnerChanged( weapon, owner )
	}
}
      