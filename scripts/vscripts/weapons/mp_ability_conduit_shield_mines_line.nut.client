global function Mp_ability_shield_mines_line_init
global function OnProjectileCollision_ability_shield_mines_line








global function GenerateMineLocations
global function GetMineRadius


const float SHIELD_MINE_ARMING_TIME = 3
const float SHIELD_MINE_LIFETIME = 60
const float SHIELD_MINE_DAMAGE_DEBOUNCE_TIME = 1.0
const float SHIELD_MINE_DAMAGE = 10
const float SHIELD_MINE_HEALTH = 250
const float SHIELD_MINE_LINE_COUNT = 7
const int SHIELD_MINE_MAX_NUM_PER_PLAYER = 14
global const string SHIELD_MINE_PROP_SCRIPTNAME = "conduit_shield_mine"
global const float SHIELD_MINE_RANGE = 200
const float SHIELD_MINE_MIN_SEPARATION = SHIELD_MINE_RANGE /2
const float SHIELD_MINE_MIN_SEPARATION_SQR = SHIELD_MINE_MIN_SEPARATION * SHIELD_MINE_MIN_SEPARATION
global const float SHIELD_MINE_AIR_TRAVEL_TIME = 1.25
global const string SHIELD_MINES_THREAT_PROP_TARGETNAME = "shield_mines_threat"

const string SHIELD_MINES_LINE_CLASS_NAME = "mp_ability_conduit_shield_mines_line"


const string SHIELD_MINE_IMPACT_SOUND = "Conduit_Ult_Impact_Default_3p"
const string SHIELD_MINE_LOOP_SOUND = "Conduit_Ult_Active_3p"
const string SHIELD_MINE_ARMING_SOUND = "Conduit_Ult_Arming_3p"
const string SHIELD_MINE_DEPLOY_SOUND = "Conduit_Ult_Jammers_Deploy_3p"

const string SHIELD_MINE_PLAYER_DMG_SOUND = "Conduit_Ult_Player_Damage_3p"
const string SHIELD_MINE_PLAYER_DMG_1P_SOUND = "Conduit_Ult_Player_Damage_1p"

const string SHIELD_MINE_DAMAGE_SPARK_SOUND = "Conduit_Ult_Jammers_Damage_3p"
const string SHIELD_MINE_DESTROY_SOUND = "Conduit_Ult_Jammers_Destroy_3p"
const string SHIELD_MINE_DESTROY_SELF_SOUND = "Conduit_Ult_Jammers_SelfDestroy_3p"


const asset SHIELD_MINE_MODEL = $"mdl/props/conduit/conduit_shield_jammer.rmdl"
const float SHIELD_MINE_MODEL_Z_OFFSET = 9
const float SHIELD_MINE_MODEL_Z_OFFSET_ARMING = 15


const asset SHIELD_MINE_PLAYER_DMG_1P_FX = $"P_con_ult_hitfx_1p"
const asset SHIELD_MINE_PLAYER_DMG_3P_FX = $"P_emp_body_human"
const asset SHIELD_MINE_PLAYER_DMG_ROPE_FX = $"P_con_ult_damageRope"


const asset SHIELD_MINES_AIR_DEPLOY_FX = $"P_con_ult_jmr_deploy"
const asset SHIELD_MINES_RADIUS_FX = $"P_con_ult_mine"
const asset SHIELD_MINES_RADIUS_ENEMY_FX = $"P_con_ult_mine_enemy"
const asset SHIELD_MINES_CONNECTION_FX = $"P_con_ult_linkRope"

const asset SHIELD_MINE_DAMAGE_SPARK_FX = $"P_tesla_trap_dmg"
const asset SHIELD_MINE_DESTROY_FX = $"P_con_ult_jmr_death"
const asset SHIELD_MINE_DESTROY_ENEMY_FX = $"P_con_ult_jmr_death_enm"
const asset SHIELD_MINE_FAIL_FX = $"P_con_ult_jmr_fail"
const string SHIELD_MINE_FX_ATTACH = "fx_center"

const asset SHIELD_MINE_CHARGE_FX = $"P_con_ult_mine_chargeUp"
const asset SHIELD_MINE_CHARGE_ENEMY_FX = $"P_con_ult_mine_chargeUp_enm"

const bool SHIELD_MINES_DEBUG = false

global const string SHIELD_MINE_BOMBARDMENT_WEAPON = "mp_ability_conduit_shield_mines_line"

struct
{




} file

void function Mp_ability_shield_mines_line_init()
{
	PrecacheParticleSystem( SHIELD_MINE_PLAYER_DMG_1P_FX )
	PrecacheParticleSystem( SHIELD_MINE_PLAYER_DMG_3P_FX )
	PrecacheParticleSystem( SHIELD_MINE_CHARGE_FX )
	PrecacheParticleSystem( SHIELD_MINE_CHARGE_ENEMY_FX )

	PrecacheParticleSystem( SHIELD_MINES_AIR_DEPLOY_FX )
	PrecacheParticleSystem( SHIELD_MINES_RADIUS_FX )
	PrecacheParticleSystem( SHIELD_MINES_RADIUS_ENEMY_FX )
	PrecacheParticleSystem( SHIELD_MINE_PLAYER_DMG_ROPE_FX )
	PrecacheParticleSystem( SHIELD_MINES_CONNECTION_FX )

	PrecacheParticleSystem( SHIELD_MINE_DAMAGE_SPARK_FX )
	PrecacheParticleSystem( SHIELD_MINE_DESTROY_FX )
	PrecacheParticleSystem( SHIELD_MINE_DESTROY_ENEMY_FX )
	PrecacheParticleSystem( SHIELD_MINE_FAIL_FX )



	PrecacheModel( SHIELD_MINE_MODEL )

	PrecacheWeapon( SHIELD_MINE_BOMBARDMENT_WEAPON )


	INVALID_GRAVITY_CANNON_PLACEABLES.append( SHIELD_MINES_LINE_CLASS_NAME )








		RegisterSignal( "PatternTargetingUI_Signal" )
		AddTargetNameCreateCallback( SHIELD_MINES_THREAT_PROP_TARGETNAME, AddThreatIndicator )


}















































float function GetMineRadius( entity owner )
{
	float result = SHIELD_MINE_RANGE








	return result
}

array<vector> function GenerateMineLocations( entity weapon, vector airBurstLocation, vector directionRight, bool DEBUG_DRAW, int projectileID = -1, bool simulateCollisions = true )
{
	array<vector> mineLocations
	array<float> radiusMultiple = [-3.0, -2, -1, 3, 2, 1, 0 ]
	Assert( radiusMultiple.len() == SHIELD_MINE_LINE_COUNT, "Array should match number of shield mines" )
	const float RADIUS_SCALAR = 1.9

	
	

	int startIndex = 0
	int endIndex = 6










	if ( projectileID != -1 )
	{
		if ( projectileID == 0 )
			endIndex = 3
		else
			startIndex = 4
	}

	for ( int i=startIndex; i<=endIndex; ++i )
	{
		
		float offsetDistance = radiusMultiple[i]*RADIUS_SCALAR*SHIELD_MINE_RANGE
		vector idealMinePos  = airBurstLocation + offsetDistance*directionRight - UP_VECTOR*SHIELD_MINE_AIRBURST_HEIGHT
		vector launchVel     = CalcProjectileTrajectory( airBurstLocation, idealMinePos, SHIELD_MINE_AIR_TRAVEL_TIME, false )

		vector minePos = idealMinePos
		if ( simulateCollisions )
		{
			minePos = weapon.SimulateGrenadeImpactPos( airBurstLocation, launchVel , -1, 3 )
		}

#if DEV
		const float DRAW_TIME = 0.1
		if ( DEBUG_DRAW )
		{
			DebugDrawSphere( airBurstLocation, 6, COLOR_GREEN, false, DRAW_TIME )
			DebugDrawSphere( idealMinePos, 6, COLOR_YELLOW, false, DRAW_TIME )
			DebugDrawSphere( minePos, 5, COLOR_GREEN, false, DRAW_TIME )
			DebugDrawText( minePos, (" Mine: " + radiusMultiple[i]), false, DRAW_TIME)
			DebugDrawArrow( airBurstLocation, minePos, 3, COLOR_LIGHT_GREEN, false, DRAW_TIME )

			vector lineEnd = airBurstLocation+ Normalize(launchVel)*30
			DebugDrawText( lineEnd, ("Proj: " + projectileID + " Mine: " + radiusMultiple[i]), false, DRAW_TIME)
			DebugDrawArrow(airBurstLocation, lineEnd, 5, COLOR_LIGHT_RED, false, DRAW_TIME)


			
			
			
			
			
			
			
			
			
			
		}
#endif
		mineLocations.append( minePos )
	}

	return mineLocations
}



void function OnProjectileCollision_ability_shield_mines_line( entity projectile, vector pos, vector normal, entity hitEnt, int hitBox, bool isCritical, bool isPassthrough )
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






	bool result = PlantStickyEntityThatBouncesOffWalls( projectile, cp, DOT_45DEGREE )

	if ( !result )
	{





















		return
	}





























	projectile.Destroy()
}











































































































































































































































































































































































































































































































































































































































































































void function AddThreatIndicator( entity grenade )
{
	
	entity player         = GetLocalViewPlayer()
	entity grenadeOwner = grenade.GetOwner()
	ShowGrenadeArrow( player, grenade, GetMineRadius( grenadeOwner )*1.4, 0.0, false, eThreatIndicatorVisibility.INDICATOR_SHOW_TO_ENEMIES, <0, 0, SHIELD_MINE_MODEL_Z_OFFSET> )
}



