

global function MpWeaponArtifactDaggerPrimary_Init

global function OnWeaponActivate_weapon_artifact_dagger_primary
global function OnWeaponDeactivate_weapon_artifact_dagger_primary
global function OnWeaponOwnerChanged_weapon_artifact_dagger


global function OnCreateVMFx_ArtifactDagger


const string ARTIFACT_MODEL_IDENTIFIER = "char_artifact"

void function MpWeaponArtifactDaggerPrimary_Init()
{
	RegisterScriptAnimWindowCallbacks( "Artifact_Dagger", ArtifactDagger_ScriptAnimWindowStartCallback, ArtifactDagger_ScriptAnimWindowStopCallback )
}

void function OnWeaponActivate_weapon_artifact_dagger_primary( entity weapon )
{
	entity owner = weapon.GetOwner()





	if ( owner.p.artifactConfig != null )
	{
		Artifacts_Loadouts_SetupWeaponComponents( weapon, owner )
		Artifacts_FX_StopWeaponFX( weapon, eArtifactFXPackageType.IDLE ) 
		Artifacts_FX_StartWeaponFX( weapon, eArtifactFXPackageType.IDLE, null )
	}
}

void function OnWeaponDeactivate_weapon_artifact_dagger_primary( entity weapon )
{
	Artifacts_FX_StopWeaponFX( weapon, eArtifactFXPackageType.IDLE )





}

void function OnWeaponOwnerChanged_weapon_artifact_dagger( entity weapon, WeaponOwnerChangedParams params )
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

void function ArtifactDagger_ScriptAnimWindowStartCallback( entity ent, string parameter )
{
	entity weapon

	if ( ent.IsWeaponX() )
		weapon = ent
	else
		weapon = ViewModel_GetWeapon( ent )

	
	if ( !IsValid( weapon ) || weapon.GetWeaponClassName() != ARTIFACT_DAGGER_MP_WEAPON )
		return

	Artifacts_FX_ScriptAnimWindowCallback( weapon, parameter, true )
}

void function ArtifactDagger_ScriptAnimWindowStopCallback( entity ent, string parameter )
{
	if ( !ent.IsWeaponX() && ent.GetModelName().find( ARTIFACT_MODEL_IDENTIFIER ) == -1 ) 
		return

	entity weapon

	if ( ent.IsWeaponX() )
		weapon = ent
	else
		weapon = ViewModel_GetWeapon( ent )

	
	if ( !IsValid( weapon ) || weapon.GetWeaponClassName() != ARTIFACT_DAGGER_MP_WEAPON )
		return

	Artifacts_FX_ScriptAnimWindowCallback( weapon, parameter, false )
}


void function OnCreateVMFx_ArtifactDagger( entity weapon, int effectHandle )
{
	Artifact_Set1pFxControlPoints( weapon, effectHandle )
}


                                  