global function MpWeaponArtifactDaggerPrimary_Init

global function OnWeaponActivate_weapon_artifact_dagger_primary
global function OnWeaponDeactivate_weapon_artifact_dagger_primary
global function OnWeaponOwnerChanged_weapon_artifact_dagger

const string ARTIFACT_MODEL_IDENTIFIER = "char_artifact"

void function MpWeaponArtifactDaggerPrimary_Init()
{
	RegisterScriptAnimWindowCallbacks( "Artifact_Dagger", ArtifactDagger_ScriptAnimWindowStartCallback, ArtifactDagger_ScriptAnimWindowStopCallback )


		AddOnSpectatorTargetChangedCallback( ArtifactDagger_OnSpectatorChanged )

}


void function ArtifactDagger_SetupSpectator( entity player, bool startFX )
{
	if ( IsValid( player ) && player.IsPlayer() )
	{
		entity weapon = player.GetActiveWeapon( eActiveInventorySlot.mainHand )

		if ( weapon != null && weapon.GetWeaponSettingBool( eWeaponVar.is_artifact ) )
		{
			if ( player.p.artifactConfig == null )
			{
				Artifacts_StoreLoadoutDataOnPlayerEntityStruct( player, weapon, false )
			}

			
			

			if ( player.p.artifactConfig != null && weapon.GetWeaponClassName() == ARTIFACT_DAGGER_MP_WEAPON )
			{
				Artifacts_Loadouts_SetupWeaponComponents( weapon, player )
				Artifacts_FX_StopWeaponFX( weapon, eArtifactFXPackageType.IDLE )
				Artifacts_FX_StopWeaponFX( weapon, eArtifactFXPackageType.ATTACK )

				if ( startFX )
				{
					Artifacts_FX_StartWeaponFX( weapon, eArtifactFXPackageType.IDLE )
					Artifacts_FX_StartWeaponFX( weapon, eArtifactFXPackageType.BLADE_EMISSIVE )
				}
			}
		}
	}
}

void function ArtifactDagger_OnSpectatorChanged( entity spectatingPlayer, entity oldSpectatorTarget, entity newSpectatorTarget )
{
	ArtifactDagger_SetupSpectator( oldSpectatorTarget, false )
	ArtifactDagger_SetupSpectator( newSpectatorTarget, true )
}


void function OnWeaponActivate_weapon_artifact_dagger_primary( entity weapon )
{
	entity owner = weapon.GetOwner()

	Melee_SetModsForLegendAbilities( owner )

	if ( owner.p.artifactConfig != null )
	{
		Artifacts_Loadouts_SetupWeaponComponents( weapon, owner )
		Artifacts_FX_StopWeaponFX( weapon, eArtifactFXPackageType.IDLE ) 
		Artifacts_FX_StopWeaponFX( weapon, eArtifactFXPackageType.ATTACK ) 
		Artifacts_FX_StartWeaponFX( weapon, eArtifactFXPackageType.IDLE )
		Artifacts_FX_StartWeaponFX( weapon, eArtifactFXPackageType.BLADE_EMISSIVE )
	}
}

void function OnWeaponDeactivate_weapon_artifact_dagger_primary( entity weapon )
{
	weapon.Signal( "WeaponDeactivateEvent" )
	Artifacts_FX_StopWeaponFX( weapon, eArtifactFXPackageType.IDLE )
	Artifacts_FX_StopWeaponFX( weapon, eArtifactFXPackageType.BLADE_EMISSIVE )





	Melee_RemoveModsForLegendAbilities( weapon.GetOwner() )
}

void function OnWeaponOwnerChanged_weapon_artifact_dagger( entity weapon, WeaponOwnerChangedParams params )
{
	entity owner = params.newOwner

	if( !IsValid( owner ) )
		return

	if ( !owner.IsBot() )
	{
		Artifacts_StoreLoadoutDataOnPlayerEntityStruct( owner, weapon, false )
		Artifacts_Loadouts_SetupWeaponComponents( weapon, owner )
		Artifacts_OnWeaponOwnerChanged( weapon, owner )
	}
}

void function ArtifactDagger_ScriptAnimWindowStartCallback( entity ent, string parameter )
{
	entity weapon

	if ( ent.IsWeaponX() )
		weapon = ent
	else if ( ent.IsPlayer() )
		weapon = ent.GetActiveWeapon( 0 )
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
	else if ( ent.IsPlayer() )
		weapon = ent.GetActiveWeapon( 0 )
	else
		weapon = ViewModel_GetWeapon( ent )

	
	if ( !IsValid( weapon ) || weapon.GetWeaponClassName() != ARTIFACT_DAGGER_MP_WEAPON )
		return

	Artifacts_FX_ScriptAnimWindowCallback( weapon, parameter, false )
}