
global function MpWeaponTitanSword_Dash_Init
global function TitanSword_Dash_OnWeaponActivate
global function TitanSword_Dash_OnWeaponDeactivate
global function TitanSword_Dash_ClearMods
global function TitanSword_Dash_TryDash
global function PlayRotatedImpactFXTable


global function ServerToClient_PlayRotatedImpactFxTable
global function ServerToClient_StopDash







const string SIG_TITAN_SWORD_DASH_ACTIVATED = "TitanSword_DashActivated"
const string SIG_TITAN_SWORD_DASH_STOPPED = "TitanSword_DashStopped"

const float TITAN_SWORD_CHARGE_DASH_SEC = 0.0
const float TITAN_SWORD_DASH_SPEED = 1400
const float TITAN_SWORD_DASH_SPEED_AIR = 950

const float TITAN_SWORD_DASH_NOT_READY_DEBOUNCE_TIME_SEC = 1
const float TITAN_SWORD_MAIN_INSTRUCTIONS_DEBOUNCE_TIME = 30 
const float TITAN_SWORD_INSTRUCTIONS_DEBOUNCE_TIME = 10


const bool TITAN_SWORD_LOS_DEBUG = false


const asset VFX_TITAN_SWORD_DASH_1P = $"P_pilot_dash_FP"
const asset VFX_TITAN_SWORD_DASH_3P = $"P_pilot_dash_3P"
const asset VFX_TITAN_SWORD_DASH_JETS = $"P_pilot_dash_launch_thruster"

const string VFX_TITAN_SWORD_DASH_START_IMPACT = "pilot_dash_start" 
const string VFX_TITAN_SWORD_DASH_SKID_IMPACT = "pilot_dash" 


const string SFX_TITAN_SWORD_DASH_1P = "titansword_special_dash_1p"
const string SFX_TITAN_SWORD_DASH_3P = "titansword_special_dash_3p"
const string SFX_TITAN_SWORD_DASH_IMPACT = "titansword_special_dash_obstacle_1p"


struct
{



}file

void function MpWeaponTitanSword_Dash_Init()
{
	RegisterImpactTable( VFX_TITAN_SWORD_DASH_START_IMPACT )
	RegisterImpactTable( VFX_TITAN_SWORD_DASH_SKID_IMPACT )

	PrecacheParticleSystem( VFX_TITAN_SWORD_DASH_1P )
	PrecacheParticleSystem( VFX_TITAN_SWORD_DASH_3P )
	PrecacheParticleSystem( VFX_TITAN_SWORD_DASH_JETS )

	RegisterSignal( SIG_TITAN_SWORD_DASH_ACTIVATED )
	RegisterSignal( SIG_TITAN_SWORD_DASH_STOPPED )


	Remote_RegisterClientFunction( "ServerToClient_PlayRotatedImpactFxTable", "entity", "vector", -MAX_MAP_BOUNDS, MAX_MAP_BOUNDS, 32, "vector", -MAX_MAP_BOUNDS, MAX_MAP_BOUNDS, 32, "int", INT_MIN, INT_MAX )
	Remote_RegisterClientFunction( "ServerToClient_StopDash" )
}

void function TitanSword_Dash_OnWeaponActivate( entity player, entity weapon )
{






}

void function TitanSword_Dash_OnWeaponDeactivate( entity player, entity weapon )
{




}

void function TitanSword_Dash_ClearMods( entity weapon )
{
	
}

void function TitanSword_Dash_StartDashing( entity player, float duration )
{

		if ( InPrediction() )
			StatusEffect_AddTimed( player, eStatusEffect.titan_sword_dash, 1.0, duration, 0.0 )



}

void function TitanSword_Dash_StopDashing( entity player )
{

		if ( InPrediction() )
			StatusEffect_StopAllOfType( player, eStatusEffect.titan_sword_dash )




	player.Signal( SIG_TITAN_SWORD_DASH_STOPPED )
}

bool function TitanSword_Dash_IsDashing( entity player )
{
	return StatusEffect_HasSeverity( player, eStatusEffect.titan_sword_dash )
}

bool function TitanSword_Dash_TryDash( entity player, entity weapon )
{
	if ( TitanSword_Block_IsBlocking( weapon ) )
	{
		if ( TitanSword_Dash_TryDashCancel( player, weapon ) && TitanSword_Heavy_TryHeavyAttack( player, weapon ) )
			return true

		
		if ( TitanSword_Dash_IsDashing( player ) )
			return true

		if ( !TitanSword_TryUseFuel( player, true ) )
			return true

		thread TitanSword_Dash_Thread( player )


			HidePlayerHint( "#WPN_TITAN_SWORD_DASH_NOT_READY" )

		return true 
	}

	return false
}

bool function TitanSword_Dash_TryDashCancel( entity player, entity weapon )
{
	if ( TitanSword_Dash_IsDashing( player ) && !weapon.IsWeaponInAds() )
	{
		return true
	}
	return false
}

void function TitanSword_Dash_Thread( entity player )
{
	
	
	
	

	if ( !IsValid( player ) )
		return

	player.Signal( SIG_TITAN_SWORD_DASH_ACTIVATED )

	player.EndSignal( SIG_TITAN_SWORD_DASH_ACTIVATED )
	player.EndSignal( "OnDeath" )
	player.EndSignal( "OnDestroy" )
	player.EndSignal( "BleedOut_OnStartDying" )
	player.EndSignal( SIG_TITAN_SWORD_DASH_STOPPED )


		if ( !InPrediction() || (InPrediction() && IsFirstTimePredicted()) )
			EmitSoundOnEntity( player, SFX_TITAN_SWORD_DASH_1P )


	float duration = 0.4

	TitanSword_Dash_StartDashing( player, duration + 0.1 )









































	
	vector fwdAdjusted = AnglesCompose( player.EyeAngles(), <90, 0, 0> )
	fwdAdjusted.x = 90

	vector eyeAngles = player.EyeAngles()
	eyeAngles.x = 0
	vector launchFwd = FlattenNormalizeVec( AnglesToForward( eyeAngles ) )

	vector normalVel = Normalize( player.GetVelocity() )

	float dashSpeed = player.IsOnGround() ? TITAN_SWORD_DASH_SPEED : TITAN_SWORD_DASH_SPEED_AIR
	
	
	
	
	
	
	vector dashVec  = launchFwd * dashSpeed
	if ( StatusEffect_HasSeverity( player, eStatusEffect.titan_sword_dash_sickness ) )
	{
		dashVec.z = player.GetVelocity().z * 0.85
	}
	player.PlayerLaunch( dashVec, false )

	
	if ( !player.IsOnGround() )
		StatusEffect_AddTimed( player, eStatusEffect.titan_sword_dash_sickness, 1.0, 5.0, 0.0 )

	thread TitanSword_Dash_PlaySkidVFX_Thread( player )

	

	
	
	
	
	
	
	

	

	wait 0.1





	wait duration

	player.Signal( SIG_TITAN_SWORD_DASH_STOPPED )
}





























void function ServerToClient_StopDash()
{
	entity player = GetLocalViewPlayer()
	if ( IsValid( player ) )
		TitanSword_Dash_StopDashing( player )
}


void function TitanSword_Dash_PlaySkidVFX_Thread( entity player )
{
	Assert ( player )
	Assert ( IsNewThread(), "Must be threaded off" )

	player.EndSignal( SIG_TITAN_SWORD_DASH_ACTIVATED )
	player.EndSignal( "OnDeath" )
	player.EndSignal( "OnDestroy" )
	player.EndSignal( SIG_TITAN_SWORD_DASH_STOPPED )
	player.EndSignal( "BleedOut_OnStartDying" )

	vector currentPos = player.GetOrigin()
	float trackDist   = 0
	vector prevPos    = currentPos
	const float SLIDE_IMPACT_FX_MIN_DIST = 20

	float startTime = Time()

	PlayRotatedImpactFXTable( player, currentPos, AnglesToForward( player.GetAngles() ), VFX_TITAN_SWORD_DASH_START_IMPACT )

	while ( TitanSword_Dash_IsDashing( player ) )
	{
		currentPos = player.GetOrigin()
		trackDist += Distance( currentPos, prevPos )

		if ( trackDist >= SLIDE_IMPACT_FX_MIN_DIST && player.IsOnGround() )
		{
			PlayRotatedImpactFXTable( player, currentPos, currentPos - prevPos, VFX_TITAN_SWORD_DASH_SKID_IMPACT )
			trackDist = 0
			prevPos   = currentPos
		}

		WaitFrame()
	}
}


void function PlayRotatedImpactFXTable( entity owner, vector origin, vector fwd, string impactFx, int flags = 0 )
{
	int impactTableIndex = GetImpactTableIndex( impactFx )

		ServerToClient_PlayRotatedImpactFxTable( owner, origin, fwd, impactTableIndex )















}


void function ServerToClient_PlayRotatedImpactFxTable( entity owner, vector origin, vector fwd, int impactTableIndex )
{
	entity localPlayer = GetLocalViewPlayer()

	if ( !IsValid( localPlayer ) )
		return

	if ( !IsValid( owner ) )
		return

	const float UP = 40
	const float DOWN = -80

	vector startPos = origin + <0, 0, UP>
	vector endPos   = origin + <0, 0, DOWN>

	TraceResults trace = TraceLineHighDetail( startPos, endPos, owner, TRACE_MASK_SHOT, TRACE_COLLISION_GROUP_NONE, owner )

	
	

	vector finalEnd = trace.endPos

	if ( !IsValid( trace.hitEnt ) ) 
	{
		if ( origin != owner.GetOrigin() ) 
		{
			startPos = owner.GetOrigin() + <0, 0, UP>
			endPos   = owner.GetOrigin() + <0, 0, -200>
			finalEnd = origin
			trace    = TraceLineHighDetail( startPos, endPos, owner, TRACE_MASK_SHOT, TRACE_COLLISION_GROUP_NONE, owner )
		}
	}

	
	

	if ( IsValid( trace.hitEnt ) )
	{
		vector normal = Normalize( startPos - (endPos + (fwd * .1)) )
		localPlayer.DispatchImpactEffects( trace.hitEnt, startPos, finalEnd, normal, trace.surfaceProp, trace.staticPropID, DMG_MELEE_ATTACK, impactTableIndex, owner, 0 )

		
		
	}
}













































































































































                               