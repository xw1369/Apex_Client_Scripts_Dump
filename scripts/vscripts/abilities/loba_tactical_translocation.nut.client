
global function LobaTacticalTranslocation_LevelInit



global function OnWeaponAttemptOffhandSwitch_ability_translocation
global function OnWeaponActivate_ability_translocation
global function OnWeaponDeactivate_ability_translocation
global function OnWeaponTossPrep_ability_translocation
global function OnWeaponToss_ability_translocation
global function OnWeaponTossReleaseAnimEvent_ability_translocation
global function OnWeaponRedirectProjectile_ability_translocation



global function ServerToClient_Translocation_ClientProjectilePlantedHandler
global function ServerToClient_Translocation_TeleportFailed










const asset TRANSLOCATION_WARP_SCREEN_FX = $"P_ability_warp_screen"
const asset TRANSLOCATION_WARP_BEAM_FX = $"P_ability_warp_travel"
const asset TRANSLOCATION_WARP_WORLD_FX = $"P_warp_imp_default"
const string TRANSLOCATION_WARP_IMPACT_TABLE = "ability_warp"
const asset TRANSLOCATION_DROP_TO_GROUND_MARKER_FX = $"P_wrp_trl_grnd"
const asset TRANSLOCATION_DROP_TO_GROUND_ACTIVATE_FX = $"P_warp_proj_drop"
const asset TRANSLOCATION_DROP_TO_GROUND_DESTINATION_FX = $"P_warp_proj_drop_grnd"

const bool TRANSLOCATION_DEBUG = false
const float TRANSLOCATION_DEBUG_TIMEOUT = 40
const int MAX_BACKUP_CANDIDATE_SPOTS = 2 
const bool TRANSLOCATION_DO_VERIFICATION = true 
const bool TRANSLOCATION_ADDITIONAL_DEBUG = false

const float RUI_MAX_RANGE = 67.0                 
const float RUI_MAX_RANGE_UPGRADED_AMOUNT = 10.0 




const bool FORCE_TELEPORT_FAIL = false

IntSet fallTriggerDamageSourceIdSet = {
	[eDamageSourceId.fall] = IN_SET,
	[eDamageSourceId.splat] = IN_SET,
	[eDamageSourceId.submerged] = IN_SET,
	[eDamageSourceId.turbine] = IN_SET,
	[eDamageSourceId.lasergrid] = IN_SET,
	[eDamageSourceId.damagedef_crush] = IN_SET,
}




enum eLobaCrosshairStage
{
	
	HELD = 0
	TOSSED = 1
	REDIRECTED = 2
	PLANTED = 3
	TELEPORTED = 4
	FAILED = 5
}



struct
{
	array<entity> canceledTeleports
}file




void function LobaTacticalTranslocation_LevelInit()
{

		PrecacheParticleSystem( TRANSLOCATION_WARP_BEAM_FX )
		PrecacheParticleSystem( TRANSLOCATION_WARP_WORLD_FX )
		PrecacheImpactEffectTable( TRANSLOCATION_WARP_IMPACT_TABLE )
		PrecacheParticleSystem( TRANSLOCATION_DROP_TO_GROUND_MARKER_FX )
		PrecacheParticleSystem( TRANSLOCATION_DROP_TO_GROUND_ACTIVATE_FX )
		PrecacheParticleSystem( TRANSLOCATION_DROP_TO_GROUND_DESTINATION_FX )

		RegisterNetworkedVariable( "Translocation_ActiveProjectile", SNDC_PLAYER_EXCLUSIVE, SNVT_ENTITY )

		Remote_RegisterClientFunction( "ServerToClient_Translocation_ClientProjectilePlantedHandler", "entity", "entity" )
		Remote_RegisterClientFunction( "ServerToClient_Translocation_TeleportFailed", "entity" )


			Remote_RegisterServerFunction( "ClientToServer_Translocation_Cancel" )


		RegisterSignal( "Translocation_Deactivate" )

		AddCallback_PlayerCanUseZipline( CanUseZipline )







		RegisterSignal( "Translocation_StopVisualEffect" )
		RegisterSignal( "Translocation_RedirectProjectile" )
		PrecacheParticleSystem( TRANSLOCATION_WARP_SCREEN_FX )
		StatusEffect_RegisterEnabledCallback( eStatusEffect.translocation_visual_effect, StartVisualEffect )
		StatusEffect_RegisterDisabledCallback( eStatusEffect.translocation_visual_effect, StopVisualEffect )


			RegisterConCommandTriggeredCallback( "+scriptCommand5", CancelTeleport )


}




bool function OnWeaponAttemptOffhandSwitch_ability_translocation( entity weapon )
{
	entity owner = weapon.GetWeaponOwner()

	if ( !IsPlayerTranslocationPermitted( owner ) )
		return false

	return true
}




void function OnWeaponActivate_ability_translocation( entity weapon )
{
	entity owner = weapon.GetWeaponOwner()
	Assert( IsValid( owner ) )








		if ( !(InPrediction() && IsFirstTimePredicted()) )
			return

		weapon.w.translocate_predictedInitialProjectile = null
		weapon.w.translocate_predictedRedirectedProjectile = null
		weapon.w.translocate_impactRumbleObj = null


	thread TranslocationLifetimeThread( owner, weapon )
}




void function OnWeaponDeactivate_ability_translocation( entity weapon )
{

#if TRANSLOCATION_ADDITIONAL_DEBUG
		entity weaponOwner = weapon.GetOwner()
		string ownerName = IsValid( weaponOwner ) ? weaponOwner.GetPlayerName() : "NULL"
#endif


		if ( !InPrediction() )
		{
#if TRANSLOCATION_ADDITIONAL_DEBUG
				printt( "TRANSLOCATION:(" + ownerName + ") Client not in prediction when deactivating weapon" )
#endif
			return
		}


#if TRANSLOCATION_ADDITIONAL_DEBUG
		printt( "TRANSLOCATION:(" + ownerName + ") Weapon deactivated" )
#endif


	if( !file.canceledTeleports.contains( weapon.GetOwner() ) )

		Signal( weapon, "Translocation_Deactivate" )
}




void function TranslocationLifetimeThread( entity owner, entity weapon )
{
	EndSignal( owner, "OnDeath" )
	EndSignal( weapon, "OnDestroy" )
	EndSignal( weapon, "Translocation_Deactivate" )

	string ownerName = IsValid( owner ) ? owner.GetPlayerName() : "NULL"

	var rui

		rui = CreateFullscreenRui( $"ui/crosshair_loba_translocation.rpak", 500 )
		RuiTrackFloat( rui, "weaponGrenadeDistToImpact", weapon, RUI_TRACK_GRENADE_DIST_FROM_IMPACT )
		RuiSetFloat( rui, "estimatedMaxDist", GetLobaTacticalEstimatedMaxDistance( owner ) )


			if( owner.HasPassive( ePassives.PAS_TAC_UPGRADE_TWO ) ) 
			{
				RuiSetString( rui, "locString", "%&attack% Drop %scriptCommand5% Cancel" )
			}



	bool[1] haveLockedForToss = [false]

	OnThreadEnd( void function() : ( owner, weapon, rui, haveLockedForToss, ownerName ) {
#if TRANSLOCATION_ADDITIONAL_DEBUG
			printt( "TRANSLOCATION:(" + ownerName + ") Translocation lifetime thread end" )
#endif

		if ( IsValid( owner ) )
		{
			if ( haveLockedForToss[0] )
			{




















			}
		}


			RuiDestroyIfAlive( rui )

	} )

	int offhandSlot = weapon.GetOffhandActiveSlot()

	while ( true )
	{
		entity currentActiveOffhandWeapon = owner.GetActiveWeapon( offhandSlot )
		if ( currentActiveOffhandWeapon != weapon )
		{
#if TRANSLOCATION_ADDITIONAL_DEBUG
				string offhandWeaponName = IsValid( currentActiveOffhandWeapon ) ? currentActiveOffhandWeapon.GetWeaponClassName() : "NULL"
				printt( "TRANSLOCATION:(" + ownerName + ") Current active weapon in the offhand slot " + offhandWeaponName + " does not match this weapon " + weapon.GetWeaponClassName() )
#endif
			break
		}


			int crosshairStage = eLobaCrosshairStage.HELD


		entity currentProjectile = GetCurrentTranslocationProjectile( owner, weapon )
		if ( IsValid( currentProjectile ) )
		{

				if ( currentProjectile.IsGrenadeStatusFlagSet( GSF_PLANTED ) )
					crosshairStage = eLobaCrosshairStage.PLANTED
				else if ( currentProjectile.IsGrenadeStatusFlagSet( GSF_REDIRECTED ) )
					crosshairStage = eLobaCrosshairStage.REDIRECTED
				else
					crosshairStage = eLobaCrosshairStage.TOSSED


			if ( !haveLockedForToss[0] )
			{



















				haveLockedForToss[0] = true
			}
		}
		else
		{

				if ( weapon.GetWeaponActivity() == ACT_VM_PICKUP )
					crosshairStage = eLobaCrosshairStage.TELEPORTED
				else if ( weapon.GetWeaponActivity() == ACT_VM_MISSCENTER )
					crosshairStage = eLobaCrosshairStage.FAILED

		}


			RuiSetInt( rui, "stage", crosshairStage )
			


		WaitFrame()
	}
}



























bool function CanUseZipline( entity player, entity zipline, vector ziplineClosestPoint )
{
	entity mainHandWeapon = player.GetActiveWeapon( eActiveInventorySlot.mainHand )
	if ( IsValid( mainHandWeapon ) && mainHandWeapon.GetWeaponClassName() == "mp_ability_translocation" )
		return GetLobaTacticalAllowZiplineWhileDeployed()

	return true
}




void function OnWeaponTossPrep_ability_translocation( entity weapon, WeaponTossPrepParams prepParams )
{
	entity owner = weapon.GetWeaponOwner()


		if ( !(InPrediction() && IsFirstTimePredicted()) )
			return


	weapon.EmitWeaponSound_1p3p( GetGrenadeDeploySound_1p( weapon ), GetGrenadeDeploySound_3p( weapon ) )


		Rumble_Play( "loba_tactical_pull", {} )

}




var function OnWeaponToss_ability_translocation( entity weapon, WeaponPrimaryAttackParams attackParams )
{
	entity owner = weapon.GetWeaponOwner()

	return weapon.GetAmmoPerShot()
}




var function OnWeaponTossReleaseAnimEvent_ability_translocation( entity weapon, WeaponPrimaryAttackParams attackParams )
{
	entity owner = weapon.GetWeaponOwner()




		if ( !(InPrediction() && IsFirstTimePredicted()) )
			return


	weapon.EmitWeaponSound_1p3p( GetGrenadeThrowSound_1p( weapon ), GetGrenadeThrowSound_3p( weapon ) )






	entity projectile = ThrowDeployable( weapon, attackParams, 1, OnProjectilePlanted, null, null )
	if ( IsValid( projectile ) )
	{
		PlayerUsedOffhand( owner, weapon, true, projectile )



















			weapon.w.translocate_predictedInitialProjectile = projectile
			


#if TRANSLOCATION_ADDITIONAL_DEBUG
			vector ownerPos = owner.GetOrigin(), ownerAng = owner.EyeAngles()
			printf( "TRANSLOCATION:(" + owner.GetPlayerName() + ") Toss from setpos %f %f %f; setang %f %f %f", ownerPos.x, ownerPos.y, ownerPos.z, ownerAng.x, ownerAng.y, ownerAng.z )
#endif

		thread TranslocationTossedThread( owner, weapon )
	}

	return weapon.GetWeaponSettingInt( eWeaponVar.ammo_per_shot ) 
}




bool function OnWeaponRedirectProjectile_ability_translocation( entity weapon, WeaponRedirectParams params )
{
	entity owner = weapon.GetWeaponOwner()

	if ( !GetLobaTacticalAllowDropToGround() )
		return false

	float dropToGroundMinimumTime = GetCurrentPlaylistVarFloat( "loba_tactical_drop_minimum_time", 0.24 )
	if ( Time() < params.projectile.GetProjectileCreationTimeServer() + dropToGroundMinimumTime )
		return false

	if ( params.projectile.HasWeaponMod( "redirect_mod" ) )
		return false






#if TRANSLOCATION_ADDITIONAL_DEBUG
		vector ownerPos = owner.GetOrigin(), ownerAng = owner.EyeAngles()
		printf( "TRANSLOCATION:(" + owner.GetPlayerName() + ") Redirect at setpos %f %f %f", params.projectilePos.x, params.projectilePos.y, params.projectilePos.z )
#endif

	weapon.StartCustomActivity( "ACT_VM_HITCENTER", WCAF_NONE )

	WeaponPrimaryAttackParams attackParams
	attackParams.pos = params.projectilePos
	attackParams.dir = <0.0, 0.0, -1.0>
	attackParams.firstTimePredicted = false
	attackParams.burstIndex = 0
	attackParams.barrelIndex = 0


	if( !PlayerHasPassive( owner, ePassives.PAS_TAC_UPGRADE_THREE ) ) 

	weapon.AddMod( "redirect_mod" )
	entity projectile = ThrowDeployable( weapon, attackParams, 1, OnProjectilePlanted, null, null )

	if( !PlayerHasPassive( owner, ePassives.PAS_TAC_UPGRADE_THREE ) ) 

	weapon.RemoveMod( "redirect_mod" )

	if ( !IsValid( projectile ) )
		return false

	projectile.AddGrenadeStatusFlag( GSF_REDIRECTED )

	string projectileSound = GetGrenadeProjectileSound( weapon )
	if ( projectileSound != "" )
		EmitSoundOnEntity( projectile, projectileSound )















		weapon.w.translocate_predictedRedirectedProjectile = projectile

		if ( !InPrediction() || IsFirstTimePredicted() )
		{
			thread DropToGroundFXThread( owner, projectile, params.projectile, params.projectilePos )
			EmitSoundOnEntity( projectile, "Loba_TeleportRing_ForceDown_3P" )
		}


	return true
}




void function TranslocationTossedThread( entity owner, entity weapon )
{
	EndSignal( owner, "OnDeath" )
	EndSignal( weapon, "OnDestroy" )
	EndSignal( weapon, "Translocation_Deactivate" )

	array<int> fxIds
	table[1] rumbleHandle = [{}]

	string ownerName = IsValid( owner ) ? owner.GetPlayerName() : "NULL"

#if TRANSLOCATION_ADDITIONAL_DEBUG
		entity ownerParent = owner.GetParent()
		if( IsValid( ownerParent ) )
		{
			printt( "TRANSLOCATION:(" + ownerName + ") parented to " + ownerParent + " when starting the toss thread" )
		}
#endif

	OnThreadEnd( void function() : ( owner, weapon, fxIds, rumbleHandle, ownerName ) {































































			
			

			CleanupFXArray( fxIds, true, false )

			rumbleHandle[0].loop = false

#if TRANSLOCATION_ADDITIONAL_DEBUG
				printt( "TRANSLOCATION: Client tossed thread ended" )
#endif

	} )


		rumbleHandle[0] = expect table(Rumble_Play( "loba_tactical_toss_loop", { loop = true, } ))
		

		int dropTargetFXId = -1
		if ( GetLobaTacticalAllowDropToGround() )
		{
			dropTargetFXId = StartParticleEffectInWorldWithHandle( GetParticleSystemIndex( TRANSLOCATION_DROP_TO_GROUND_MARKER_FX ),
				weapon.GetAttackPosition(), <-90, VectorToAngles( weapon.GetAttackDirection() ).y, 0> )
			if ( dropTargetFXId != -1 )
				fxIds.append( dropTargetFXId )
		}


	bool didDrop        = false
	bool dropInProgress = false

	float tossTime     = Time()
	float timeoutDelay = weapon.GetWeaponSettingFloat( eWeaponVar.grenade_fuse_time )




	
	
	while( true )
	{
		entity currentProjectile = GetCurrentTranslocationProjectile( owner, weapon )
		if ( !IsValid( currentProjectile ) )
		{
#if TRANSLOCATION_ADDITIONAL_DEBUG
				printt( "TRANSLOCATION:(" + ownerName + ") Projectile was invalid in translocation tossed thread" )
#endif
			break
		}




























			if ( dropTargetFXId != 1 )
				EffectSetControlPointVector( dropTargetFXId, 0, OriginToGround( currentProjectile.GetOrigin(), TRACE_MASK_NPCWORLDSTATIC, currentProjectile ) + <0, 0, 5> )


		
		if ( !IsPlayerTranslocationPermitted( owner ) )
			break

		

		WaitFrame()
	}










}
























































































void function OnProjectilePlanted( entity projectile, DeployableCollisionParams collisionParams )
{
	entity owner = projectile.GetOwner()
	if ( !IsValid( owner ) )
		return

	entity weapon = projectile.GetWeaponSource()
	if ( !IsValid( weapon ) )
		return


	if( file.canceledTeleports.contains( owner ) )
	{



		thread CleanupCanceledTeleport( owner )
		return
	}












#if TRANSLOCATION_ADDITIONAL_DEBUG
			printt( "TRANSLOCATION: Client projectile had predicted plant" )
#endif
		ClientProjectilePlantHandler( weapon, projectile ) 

}




void function CleanupCanceledTeleport( entity player )
{
	wait( .1 )
	if( file.canceledTeleports.contains( player ) )
		file.canceledTeleports.fastremovebyvalue( player )
}




void function ServerToClient_Translocation_ClientProjectilePlantedHandler( entity weapon, entity projectile )
{
	if ( !IsValid( weapon ) || !IsValid( projectile ) )
		return

	ClientProjectilePlantHandler( weapon, projectile )
}




void function ClientProjectilePlantHandler( entity weapon, entity projectile )
{
	

	if ( weapon.w.translocate_impactRumbleObj == null )
		weapon.w.translocate_impactRumbleObj = expect table(Rumble_Play( "loba_tactical_impact_and_teleport", {} ))

#if TRANSLOCATION_ADDITIONAL_DEBUG
		printt( "TRANSLOCATION: Client Projectile planted" )
#endif
}




void function ServerToClient_Translocation_TeleportFailed( entity weapon )
{
#if TRANSLOCATION_ADDITIONAL_DEBUG
		printt( "TRANSLOCATION: Client teleport failed" )
#endif

	if ( !IsValid( weapon ) )
		return

	if ( weapon.w.translocate_impactRumbleObj == null )
		return

	table impactRumbleObj = expect table(weapon.w.translocate_impactRumbleObj)
	impactRumbleObj.scale = 0.0
	weapon.w.translocate_impactRumbleObj = null
}
















































































































































































































































































































































































bool function IsPlayerTranslocationPermitted( entity player )
{
	if ( IsValid( player.GetParent() ) && !player.IsPlayerInAnyVehicle() )
		return false

	if ( IsPlayingFirstPersonAnimation( player ) )
		return false

	if ( IsPlayingFirstAndThirdPersonAnimation( player ) )
		return false

	if ( player.IsPhaseShifted() )
		return false

	if ( Bleedout_IsBleedingOut( player ) )
		return false

	bool allowDeployWhileZiplining = GetLobaTacticalAllowDeployWhileZiplining()
	bool allowZiplineWhileDeployed = GetLobaTacticalAllowZiplineWhileDeployed()
	bool isZiplining               = player.ContextAction_IsZipline()

	if ( IsBitFlagSet( player.GetWeaponDisableFlags(), WEAPON_DISABLE_FLAGS_MAIN ) )
	{
		if ( allowZiplineWhileDeployed && isZiplining )
		{
			
			
		}
		else
		{
			return false
		}
	}

	if ( player.ContextAction_IsActive() )
	{
		if ( (allowDeployWhileZiplining || allowZiplineWhileDeployed) && isZiplining )
		{
			
			
		}
		else
		{
			return false
		}
	}

	if ( player.ContextAction_IsMeleeExecution() || player.ContextAction_IsMeleeExecutionTarget() )
		return false

	return true
}






































void function DropToGroundFXThread( entity player, entity existingProjectile, entity predictedRedirectedProjectile, vector currentProjectilePos )
{
	if ( !IsValid( predictedRedirectedProjectile ) || predictedRedirectedProjectile.IsMarkedForDeletion() )
		return

	TraceResults tr = TraceLineHighDetail( currentProjectilePos, currentProjectilePos - <0, 0, 2500>,
		[ existingProjectile, predictedRedirectedProjectile ], TRACE_MASK_SHOT, TRACE_COLLISION_GROUP_NONE, predictedRedirectedProjectile )

	FXHandle flashFX = StartEntityFXWithHandle( predictedRedirectedProjectile, TRANSLOCATION_DROP_TO_GROUND_ACTIVATE_FX, FX_PATTACH_ABSORIGIN_FOLLOW, ATTACHMENTID_INVALID )

	FXHandle lineFX = StartWorldFXWithHandle( TRANSLOCATION_DROP_TO_GROUND_DESTINATION_FX, currentProjectilePos, <0, predictedRedirectedProjectile.GetAngles().y, 0> )




	EffectSetControlPointVector( lineFX, 1, tr.endPos )

	OnThreadEnd( function () : ( flashFX, lineFX ) {
		CleanupFXHandle( flashFX )
		CleanupFXHandle( lineFX )
	} )

	wait 2.0
}




void function StartVisualEffect( entity player, int statusEffect, bool actuallyChanged )
{
	if ( player != GetLocalViewPlayer() || (GetLocalViewPlayer() == GetLocalClientPlayer() && !actuallyChanged) )
		return

	thread (void function() : ( player, statusEffect ) {
		EndSignal( player, "OnDeath" )
		EndSignal( player, "Translocation_StopVisualEffect" )

		int fxHandle = StartParticleEffectOnEntityWithPos( player,
			GetParticleSystemIndex( TRANSLOCATION_WARP_SCREEN_FX ),
			FX_PATTACH_ABSORIGIN_FOLLOW, ATTACHMENTID_INVALID, player.EyePosition(), <0, 0, 0> )

		EffectSetIsWithCockpit( fxHandle, true )

		OnThreadEnd( function() : ( fxHandle ) {
			CleanupFXHandle( fxHandle, false, true )
		} )

		while( true )
		{
			if ( !EffectDoesExist( fxHandle ) )
				break

			float severity = StatusEffect_GetSeverity( player, statusEffect )
			
			EffectSetControlPointVector( fxHandle, 1, <severity, 999, 0> )

			WaitFrame()
		}
	})()
}




entity function GetCurrentTranslocationProjectile( entity owner, entity weapon )
{












		if ( IsValid( weapon.w.translocate_predictedRedirectedProjectile ) )
		{
			return weapon.w.translocate_predictedRedirectedProjectile
		}
		else if ( IsValid( weapon.w.translocate_predictedInitialProjectile ) )
		{
			return weapon.w.translocate_predictedInitialProjectile
		}
		else
		{
			entity serverProjectile = owner.GetPlayerNetEnt( "Translocation_ActiveProjectile" )
			if ( IsValid( serverProjectile ) )
				return serverProjectile
		}


	return null
}





void function CancelTeleport( entity player )
{
	entity offhandWeapon = player.GetOffhandWeapon( OFFHAND_TACTICAL )
	if( !IsValid( offhandWeapon ) )
		return
	if( !IsValid( GetCurrentTranslocationProjectile( player, offhandWeapon ) ) )
		return

	if( !file.canceledTeleports.contains( player ) )
		file.canceledTeleports.append( player )

	if( !PlayerHasPassive( player, ePassives.PAS_TAC_UPGRADE_TWO ) ) 
		return

	Remote_ServerCallFunction( "ClientToServer_Translocation_Cancel" )
}


void function StopVisualEffect( entity player, int statusEffect, bool actuallyChanged )
{
	if ( player != GetLocalViewPlayer() || (GetLocalViewPlayer() == GetLocalClientPlayer() && !actuallyChanged) )
		return

	player.Signal( "Translocation_StopVisualEffect" )
}




float function GetLobaTacticalEstimatedMaxDistance( entity owner )
{
	float result = RUI_MAX_RANGE

	if ( UpgradeCore_IsEnabled() && PlayerHasPassive( owner, ePassives.PAS_TAC_UPGRADE_ONE ) )
	{
		result += RUI_MAX_RANGE_UPGRADED_AMOUNT
		return GetCurrentPlaylistVarFloat( "loba_tactical_upgraded_estimated_max_distance", result )
	}

	return GetCurrentPlaylistVarFloat( "loba_tactical_estimated_max_distance", result )
}




bool function GetLobaTacticalAllowDropToGround()
{
	return GetCurrentPlaylistVarBool( "loba_tactical_allow_drop_to_ground", true )
}




bool function GetLobaTacticalAllowMantle()
{
	return GetCurrentPlaylistVarBool( "loba_tactical_allow_mantle", true )
}




bool function GetLobaTacticalAllowZiplineWhileDeployed()
{
	return GetCurrentPlaylistVarBool( "loba_tactical_allow_zipline_while_deployed", false )
}




bool function GetLobaTacticalAllowDeployWhileZiplining()
{
	return GetCurrentPlaylistVarBool( "loba_tactical_allow_deploy_while_ziplining", true )
}

