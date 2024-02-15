global function Mp_ability_shield_mines_init
global function OnProjectileCollision_ability_shield_mines
global function OnWeaponPrimaryAttack_ability_shield_mines
global function OnWeaponActivate_ability_shield_mines
global function OnWeaponDeactivate_ability_shield_mines
global function OnWeaponOwnerChanged_ability_shield_mines

global function OnWeaponAttemptOffhandSwitch_ability_shield_mines

#if DEV
global function DEV_ShieldMineLaunchDebug


const bool SHIELD_MINES_POSE_PARAM_DEBUG = false

#endif

enum eDebugStage
{
	INITIAL_TARGET_AIR_POS,
	SIMULATED_ARC_END_POS,
	DRAW_GENERATED_MINE_LOCS,
	OFF
}
int SHIELD_MINES_AIRBURST_DEBUG = eDebugStage.OFF


global const string SHIELD_MINES_WEAPON_NAME = "mp_ability_conduit_shield_mines"


const string SHIELD_MINE_IMPACT_SOUND = "Conduit_Ult_Impact_Default_3p"

const asset FX_SHIELD_MINE_PREVIEW = $"P_ar_ping_squad_CP"
const asset FX_SHIELD_MINE_PREVIEW_CHEAP = $"P_ar_ping_squad_cheap_CP"
const asset FX_SHIELD_MINE_RING_PREVIEW = $"P_ar_target_fuse_instant"
const asset FX_SHIELD_MINE_PROJ = $"P_con_ult_proj"


const float SHIELD_MINE_MAX_ATTACK_RANGE = 2000.0 
const float SHIELD_MINE_MIN_ATTACK_RANGE = 120.0 

global const float SHIELD_MINE_AIRBURST_HEIGHT = 4 * METERS_TO_INCHES

const float SHIELD_MINES_AIM_PITCH_ANGLE_MIN = -25.0
const float SHIELD_MINES_AIM_PITCH_ANGLE_MAX = 50.0
const float SHIELD_MINES_AIM_PITCH_PARAM_MIN = 0.0
const float SHIELD_MINES_AIM_PITCH_PARAM_MAX = 80.0
const float SHIELD_MINES_AIM_PITCH_DIFF_SNAP_VALUE = 1.0
const float SHIELD_MINES_AIM_PITCH_DIFF_CHECK_MIN = -90.0
const float SHIELD_MINES_AIM_PITCH_DIFF_CHECK_MAX = 90.0
const float SHIELD_MINES_AIM_PITCH_INCREMENT_MIN = -7
const float SHIELD_MINES_AIM_PITCH_INCREMENT_MAX = 7


struct
{



	table< entity, vector > cachedImpactPos
} file

void function Mp_ability_shield_mines_init()
{
	PrecacheParticleSystem( FX_SHIELD_MINE_PREVIEW )
	PrecacheParticleSystem( FX_SHIELD_MINE_PREVIEW_CHEAP )
	PrecacheParticleSystem( FX_SHIELD_MINE_RING_PREVIEW )
	PrecacheParticleSystem( FX_SHIELD_MINE_PROJ )


		RegisterSignal( "ShieldMines_ArcPreviewStop" )

}


void function OnWeaponActivate_ability_shield_mines( entity weapon )
{
	entity weaponOwner = weapon.GetWeaponOwner()
	if ( !IsValid(weaponOwner) )
		return






			if ( weaponOwner != GetLocalViewPlayer() )
				return

			thread WeaponArcPreviewThread_Client( weaponOwner, weapon )

}


void function OnWeaponDeactivate_ability_shield_mines( entity weapon )
{

	entity owner = weapon.GetOwner()
	if ( IsValid(owner) )
		owner.Signal( "ShieldMines_ArcPreviewStop" )
















}

void function OnWeaponOwnerChanged_ability_shield_mines( entity weapon, WeaponOwnerChangedParams changeParams )
{














}

bool function OnWeaponAttemptOffhandSwitch_ability_shield_mines( entity weapon )
{
	bool hasFullAmmo = weapon.GetWeaponPrimaryClipCount() >= weapon.GetWeaponPrimaryClipCountMax()


	if( !hasFullAmmo )
	{
		entity player = weapon.GetOwner()
		if ( IsValid( player ) )
			EmitSoundOnEntity( player, "Survival_UI_Ability_NotReady" )
	}


	return hasFullAmmo
}


var function OnWeaponPrimaryAttack_ability_shield_mines( entity weapon, WeaponPrimaryAttackParams attackParams )
{
	entity weaponOwner            = weapon.GetWeaponOwner()
	if ( !IsValid( weaponOwner ) )
		return


	if ( !weapon.ShouldPredictProjectiles() )
		return

	weaponOwner.Signal( "ShieldMines_ArcPreviewStop" )


	bool projectilePredicted      = PROJECTILE_PREDICTED
	bool projectileLagCompensated = PROJECTILE_LAG_COMPENSATED









	
	vector impactPos
	if ( attackParams.burstIndex == 0 )
	{
		impactPos = GetFinalImpactPos( weapon, weaponOwner )
		file.cachedImpactPos[weaponOwner] <- impactPos
	}
	else if ( attackParams.burstIndex == 1 )
	{
		if ( weaponOwner in file.cachedImpactPos )
			impactPos = file.cachedImpactPos[weaponOwner]
		else
		{
			ReportNonFatalErrorMsg( "Conduit Ult - somehow firing the second projectile without a cached impact position from the first one." )
			impactPos = GetFinalImpactPos( weapon, weaponOwner )
		}
	}

	TraceResults trUp = TraceLine( impactPos, impactPos + <0,0,SHIELD_MINE_AIRBURST_HEIGHT>, [ weaponOwner ], TRACE_MASK_GRENADE, TRACE_COLLISION_GROUP_PROJECTILE )
	vector finalTargetPos = trUp.endPos
	vector savedDeployPos = finalTargetPos

	
	vector fireDirection = FlattenNormalizeVec( attackParams.dir )
	vector fireAngles = VectorToAngles( fireDirection )
	vector fireRightDir = AnglesToRight( fireAngles )
	vector offset = -fireRightDir*30
	if ( attackParams.burstIndex == 0 )
		offset *= -1

	finalTargetPos += offset
	
	




















	
	float launchSpeed = weapon.GetWeaponSettingFloat( eWeaponVar.projectile_launch_speed )
	ArcSolution as = SolveBallisticArc( weapon.GetAttackPosition(), launchSpeed*1.05, finalTargetPos, GetConVarFloat( "sv_gravity" ) )
	
	

	entity grenade = Grenade_Launch( weapon, attackParams.pos, as.fire_velocity/launchSpeed, projectilePredicted, projectileLagCompensated, ZERO_VECTOR )






	thread BombFlightThread( grenade, weapon, weaponOwner, finalTargetPos)


		entity owner = weapon.GetOwner()
		if ( IsValid(owner) )
			weaponOwner.Signal( "ShieldMines_ArcPreviewStop" )



	int ammoUsed = weapon.GetAmmoPerShot()
	return ammoUsed
}

void function BombFlightThread( entity projectile, entity weapon, entity player, vector target )
{
	EndSignal( projectile, "OnDestroy" )
	EndSignal( player, "OnDestroy" )

	vector launchDirection = player.GetViewForward()








	vector projectileOriginStart = projectile.GetOrigin()
	float startDistance = Distance2D( projectileOriginStart, target )

	while ( true )
	{
		vector projectileOrigin = projectile.GetOrigin()
		vector projectileOriginXY = FlattenVec( projectileOrigin )
		vector projectileDir = Normalize( projectile.GetVelocity() )
		projectile.SetAngles( VectorToAngles( projectileDir ) )


























		WaitFrame()
	}
	unreachable
}


void function OnProjectileCollision_ability_shield_mines( entity projectile, vector pos, vector normal, entity hitEnt, int hitBox, bool isCritical, bool isPassthrough )
{
	entity player = projectile.GetOwner()
	if ( hitEnt == player )
		return

	
	

	DeployableCollisionParams cp
	cp.pos        = pos
	cp.normal     = normal
	cp.hitEnt     = hitEnt
	cp.hitBox     = hitBox
	cp.isCritical = isCritical

		cp.deployableFlags = eDeployableFlags.VEHICLES_NO_STICK





	

	
	



















}



void function WeaponArcPreviewThread_Client( entity owner, entity weapon )
{

	owner.EndSignal( "ShieldMines_ArcPreviewStop" )
	owner.EndSignal( "OnDestroy" )
	weapon.EndSignal( "OnDestroy" )

	while ( !IsValid( owner.GetOffhandWeapon( OFFHAND_ORDNANCE ) ) )
		WaitFrame()

	const vector DEFAULT_COLOR = < 50, 200, 255>
	const float MORTAR_RING_RADIUS_FX_DIVISOR = 20.0

	vector initialImpactPos = weapon.GetMostRecentGrenadeImpactPos()

	int ringFX = StartParticleEffectInWorldWithHandle( GetParticleSystemIndex( FX_SHIELD_MINE_RING_PREVIEW ), ZERO_VECTOR, ZERO_VECTOR )
	EffectSetControlPointVector( ringFX, 0, initialImpactPos )
	EffectSetControlPointVector( ringFX, 1, DEFAULT_COLOR )
	EffectSetControlPointVector( ringFX, 2, <GetMineRadius( owner ) / MORTAR_RING_RADIUS_FX_DIVISOR, 0, 0> )


	int airBurstMarkerFX 
	

	int centerMarkerFX = StartParticleEffectInWorldWithHandle( GetParticleSystemIndex( FX_SHIELD_MINE_PREVIEW ), ZERO_VECTOR, ZERO_VECTOR )
	EffectSetControlPointVector( centerMarkerFX, 0, initialImpactPos )
	EffectSetControlPointVector( centerMarkerFX, 1, FRIENDLY_COLOR_FX )

	array<int> sideMarkerHandles
	int maxMarkers = 6

	if( PlayerHasPassive( owner, ePassives.PAS_ULT_UPGRADE_ONE ) )
		maxMarkers = 8

	for (int i=0; i<maxMarkers; ++i )
	{
		int sideMarkerFX = StartParticleEffectInWorldWithHandle( GetParticleSystemIndex( FX_SHIELD_MINE_PREVIEW_CHEAP ), ZERO_VECTOR, ZERO_VECTOR )
		sideMarkerHandles.append( sideMarkerFX )
		EffectSetControlPointVector( sideMarkerFX, 0, initialImpactPos )
		EffectSetControlPointVector( sideMarkerFX, 1, FRIENDLY_COLOR_FX )

	}

	array<int> sideRingHandles
	
	
	
	
	
	
	
	
	

	var overlayRui = CreateCockpitPostFXRui( $"ui/shield_mines_placement.rpak", HUD_Z_BASE )
	RuiSetVisible( overlayRui, true )

	OnThreadEnd(
		function() : ( owner, ringFX, airBurstMarkerFX, centerMarkerFX, weapon, sideMarkerHandles, sideRingHandles, overlayRui )
		{
			if( EffectDoesExist( ringFX ) )
				EffectStop( ringFX, true, false )

			if( EffectDoesExist( airBurstMarkerFX ) )
				EffectStop( airBurstMarkerFX, true, false )

			if( EffectDoesExist( centerMarkerFX ) )
				EffectStop( centerMarkerFX, true, false )

			if( IsValid( weapon ) )
				weapon.ClearIndicatorEffectOverrides()

			foreach( sideMarker in sideMarkerHandles )
			{
				if( EffectDoesExist( sideMarker ) )
					EffectStop( sideMarker, true, false )
			}
			foreach( sideRing in sideRingHandles )
			{
				if( EffectDoesExist( sideRing ) )
					EffectStop( sideRing, true, false )
			}

			RuiDestroyIfAlive( overlayRui )

			
		}
	)

	while( true )
	{

		vector impactPos = GetFinalImpactPos( weapon, owner, SHIELD_MINES_AIRBURST_DEBUG == eDebugStage.INITIAL_TARGET_AIR_POS )
		TraceResults trUp = TraceLine( impactPos, impactPos + <0,0,SHIELD_MINE_AIRBURST_HEIGHT>, [ owner ], TRACE_MASK_SHOT_BRUSHONLY, TRACE_COLLISION_GROUP_NONE )

		vector finalTargetPos = impactPos
		const float DOWN_OFFSET = 10
		if ( trUp.endPos.z - impactPos.z > DOWN_OFFSET*2 )
			finalTargetPos = trUp.endPos - <0,0,DOWN_OFFSET>

		float distanceToTarget = Distance( weapon.GetAttackPosition(), impactPos )

#if DEV
		if ( SHIELD_MINES_AIRBURST_DEBUG == eDebugStage.INITIAL_TARGET_AIR_POS )
		{
			DebugDrawSphere( impactPos, 5, COLOR_RED, false, 0.1 )
			DebugDrawLine( impactPos, finalTargetPos, COLOR_RED, false, 0.1 )
			DebugDrawSphere( finalTargetPos, 10, COLOR_RED, false, 0.1 )
		}
#endif


		vector arcEndPos                  = finalTargetPos
		vector arcEndNormal = ZERO_VECTOR


		vector flattenedAttackDir = FlattenNormalizeVec(weapon.GetAttackDirection())
		vector fwdAngles = VectorToAngles( flattenedAttackDir )
		
		
		
		vector directionRight = AnglesToRight( fwdAngles )
		array<float> radiusMultiple = [1.0,-1,2,-2,3,-3]
		array<vector> sideMarkerPositions


		
		
		

		vector centerMinePos = impactPos

		float launchSpeed  = weapon.GetWeaponSettingFloat( eWeaponVar.projectile_launch_speed )
		
		ArcSolution as = SolveBallisticArc( weapon.GetAttackPosition(), launchSpeed*1.05, finalTargetPos, GetConVarFloat( "sv_gravity" ) )
		
		
		
		
		if ( !as.valid )
		{
			
			WaitFrame()
			continue
		}


		weapon.SetIndicatorEffectVelocityOverride( as.fire_velocity )
		weapon.SetIndicatorEffectDurationOverride( as.duration )
		arcEndPos = weapon.SimulateGrenadeImpactPos( ZERO_VECTOR, as.fire_velocity, as.duration, -1 )
		
		
		
		
		
		
		
		
		
		
		
		

#if DEV
		if ( SHIELD_MINES_AIRBURST_DEBUG == eDebugStage.SIMULATED_ARC_END_POS )
		{
			DebugDrawSphere( arcEndPos, 8, COLOR_GREEN, false, 0.1 )
			
		}
#endif

		entity shieldMineLineWeapon = owner.GetOffhandWeapon( OFFHAND_ORDNANCE )
		if ( !IsValid(shieldMineLineWeapon ) )
		{
			WaitFrame()
			continue
		}

		
		
		vector aimDirection = VectorRotateAxis( owner.GetViewForward(),owner.GetViewRight(), 15)
		float dot = DotProduct( aimDirection , Normalize( as.fire_velocity ) )
		float angle = DotToAngle( dot )

		vector fireVelAngles = VectorToAngles( Normalize( as.fire_velocity ) )
		vector fireUp = AnglesToUp( fireVelAngles )
		float dotUp = DotProduct( fireUp, aimDirection )

		angle *= dotUp > 0.0 ? -1.0 : 1.0

		float desiredAimPitch = GraphCapped( angle, SHIELD_MINES_AIM_PITCH_ANGLE_MIN, SHIELD_MINES_AIM_PITCH_ANGLE_MAX, SHIELD_MINES_AIM_PITCH_PARAM_MIN, SHIELD_MINES_AIM_PITCH_PARAM_MAX  )

		vector start = owner.CameraPosition() + (owner.GetViewForward() * 10) + (owner.GetViewRight() * -5)

#if DEV
		if ( SHIELD_MINES_POSE_PARAM_DEBUG )
		{
			DebugDrawArrow( start, start + (aimDirection * 20), 2, COLOR_GREEN, false, 0.1 )

			DebugDrawArrow( start, start + (Normalize( as.fire_velocity ) * 20), 2, COLOR_YELLOW, false, 0.1 )
			DebugDrawLine( start, start + (fireUp * 20), COLOR_YELLOW, false, 0.1 )

			DebugDrawLine( start, start + (FlattenNormalizeVec( owner.GetViewForward() ) * 20), COLOR_RED, false, 0.1 )
			DebugDrawLine( start, start + (UP_VECTOR * 20), COLOR_RED, false, 0.1 )

			printt( "dot " + dot + " angle " + angle + " desiredAimPitch " + desiredAimPitch )
		}
#endif
		DoPoseParamLerp( weapon, desiredAimPitch, false )
		

		array<vector> mineLocations = GenerateMineLocations( shieldMineLineWeapon, arcEndPos, directionRight, SHIELD_MINES_AIRBURST_DEBUG == eDebugStage.DRAW_GENERATED_MINE_LOCS )

#if DEV
		
		
		
		
		
		
		
#endif


		sideMarkerPositions = mineLocations
		
		
		

		
		
		
		
		
		
		
		



		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		

		
		

		
		
		
		
		
		
		
		
		
		
		
		
		


		
		
		
		

		if( EffectDoesExist( ringFX ) )
			EffectSetControlPointVector( ringFX, 0, centerMinePos )
		if( EffectDoesExist( airBurstMarkerFX ) )
			EffectSetControlPointVector( airBurstMarkerFX, 0, centerMinePos - <0,0,450> )
		if( EffectDoesExist( centerMarkerFX ) )
			EffectSetControlPointVector( centerMarkerFX, 0, centerMinePos )


		for( int i=0; i<sideMarkerHandles.len(); i++ )
		{
			int markerHandle = sideMarkerHandles[i]
			if( EffectDoesExist( markerHandle ) )
			{
				EffectWake( markerHandle )
				EffectSetControlPointVector( markerHandle, 0, sideMarkerPositions[i] )
			}
			
			
			
			
			
			
		}

		WaitFrame()
	}

}

void function DoPoseParamLerp( entity weapon, float target, bool immediate )
{
	if( !immediate )
	{
		float currentAimPitch = weapon.GetScriptPoseParam0()

		float diff = target - currentAimPitch
		float aimPitchIncrement = GraphCapped( diff, SHIELD_MINES_AIM_PITCH_DIFF_CHECK_MIN, SHIELD_MINES_AIM_PITCH_DIFF_CHECK_MAX, SHIELD_MINES_AIM_PITCH_INCREMENT_MIN, SHIELD_MINES_AIM_PITCH_INCREMENT_MAX )
		float aimPitch = 0
		if( fabs( diff ) < SHIELD_MINES_AIM_PITCH_DIFF_SNAP_VALUE )
			aimPitch = target
		else
			aimPitch = currentAimPitch + aimPitchIncrement

		weapon.SetScriptPoseParam0( aimPitch )
	}
	else
	{
		weapon.SetScriptPoseParam0( target )
	}
}



vector function GetFinalImpactPos( entity weapon, entity player, bool DEBUG_DRAW = false )
{
	vector impactPos = weapon.SimulateGrenadeImpactPos( ZERO_VECTOR, ZERO_VECTOR, -1, -1 )
	vector flattenedAttackDir = FlattenNormalizeVec( weapon.GetAttackDirection() )

	TraceResults trImpact = TraceLine( impactPos, impactPos + flattenedAttackDir*1 * METERS_TO_INCHES, [player],TRACE_MASK_SHOT_BRUSHONLY, TRACE_COLLISION_GROUP_NONE )
	if ( DEBUG_DRAW )
	{
		DebugDrawLine( impactPos, trImpact.endPos, trImpact.fraction < 1.0 ? COLOR_RED : COLOR_GREEN, false, 0.1 )
		if ( trImpact.fraction < 1.0 )
		{
			DebugDrawArrow( trImpact.endPos, trImpact.endPos +(trImpact.surfaceNormal*10), 5, COLOR_LIGHT_RED, false, 0.1)
			float upDot = DotProduct( trImpact.surfaceNormal, UP_VECTOR )
			float upDotAbs  = fabs( upDot )
			bool normalIsFlat = upDotAbs < DOT_45DEGREE
			DebugDrawText( trImpact.endPos, normalIsFlat ? "Wall" : "Ground", false, 0.1 )
		}
	}
	if ( trImpact.fraction < 1.0 )
	{
		float upDot = DotProduct( trImpact.surfaceNormal, UP_VECTOR )
		float upDotAbs  = fabs( upDot )
		bool normalIsFlat = upDotAbs < DOT_45DEGREE

		if ( normalIsFlat )
		{
			const float LEDGE_CHECK_UP = SHIELD_MINE_AIRBURST_HEIGHT
			const float LEDGE_CHECK_BACK = 1 * METERS_TO_INCHES
			float debugDrawTime      = DEBUG_DRAW ? 0.1 : 0.0
			WallToTopResults results = TraceFromWallToTop( impactPos, -flattenedAttackDir, [ player ], LEDGE_CHECK_BACK, LEDGE_CHECK_UP, TRACE_MASK_SHOT_BRUSHONLY, TRACE_COLLISION_GROUP_NONE, debugDrawTime )
			if ( results.found )
			{
				impactPos = results.pos
			}
		}
	}

	return impactPos
}


float function GetShieldMineUpgradedRangeScaler()
{
	return GetCurrentPlaylistVarFloat( "upgrade_shield_min_range_scaler", 1.15 )
}


float function GetShieldMineMaxRange( entity player )
{
	float result = SHIELD_MINE_MAX_ATTACK_RANGE


	if( PlayerHasPassive( player, ePassives.PAS_ULT_UPGRADE_ONE ) )
	{
		result *= GetShieldMineUpgradedRangeScaler()
	}


	return result
}

#if DEV
void function DEV_ShieldMineLaunchDebug( int stage = -1 )
{
	int desiredStage = (SHIELD_MINES_AIRBURST_DEBUG + 1)
	if ( stage >= 0 )
		desiredStage = stage


	SHIELD_MINES_AIRBURST_DEBUG = desiredStage % (eDebugStage.OFF + 1 )
	printt( "Conduit ShieldMine Debug state " + SHIELD_MINES_AIRBURST_DEBUG )
}
#endif
