
global function MpWeaponTitanSword_Launcher_Init
global function TitanSword_Launcher_OnWeaponActivate
global function TitanSword_Launcher_ClearMods
global function TitanSword_Launcher_TryLauncher
global function TitanSword_Launcher_VictimHitOverride
global function TitanSword_Launcher_TryLauncherAnimEvent


global const string TITAN_SWORD_LAUNCHER_MOD = "launcher"










const asset VFX_TITAN_SWORD_LAUNCHER_JETS = $"P_pilot_dash_launch_thruster"
const string VFX_TITAN_SWORD_LAUNCHER_TAKEOFF_IMPACT = "pilot_bodyslam"
const TITAN_SWORD_FX_LAUNCH_ATK_FP = $"P_pilot_sword_swipe_launch_FP"
const TITAN_SWORD_FX_LAUNCH_ATK_3P = $"P_pilot_sword_swipe_launch_3P"




const string SFX_TITAN_SWORD_LAUNCH_1P = "titansword_special_launch_1p"

struct
{
}file

void function MpWeaponTitanSword_Launcher_Init()
{
	PrecacheParticleSystem( VFX_TITAN_SWORD_LAUNCHER_JETS )

	PrecacheParticleSystem( TITAN_SWORD_FX_LAUNCH_ATK_FP )
	PrecacheParticleSystem( TITAN_SWORD_FX_LAUNCH_ATK_3P )






}

void function TitanSword_Launcher_OnWeaponActivate( entity player, entity weapon )
{



}

void function TitanSword_Launcher_StartVFX( entity weapon )
{
	
	weapon.PlayWeaponEffect( TITAN_SWORD_FX_LAUNCH_ATK_FP, TITAN_SWORD_FX_LAUNCH_ATK_3P, "blade_mid" )
}

void function TitanSword_Launcher_ClearMods( entity weapon )
{
	weapon.RemoveMod( TITAN_SWORD_LAUNCHER_MOD )
}













bool function TitanSword_Launcher_TryLauncher( entity player, entity weapon )
{

		if ( !InPrediction() || !IsFirstTimePredicted() )
			return false


	if ( !player.IsOnGround() )
		return false

	if ( !TitanSword_TryUseFuel( player ) )
		return false

	printt( "TRYING LAUNCHER" )

	TitanSword_SafelyAddAttackMod( weapon, TITAN_SWORD_LAUNCHER_MOD )

	TitanSword_Launcher_StartVFX( weapon )

	thread Launcher_Thread( player, weapon )
	

	return true
}

void function Launcher_Thread( entity player, entity weapon )
{
	player.EndSignal( "OnDeath" )
	player.EndSignal( "OnDestroy" )

	weapon.EndSignal( "OnDestroy" )
	
	weapon.EndSignal( SIG_TITAN_SWORD_DEACTIVATE )


	if ( TitanSword_ClientPredictCheck( "launcher" ) )
	{
		EmitSoundOnEntity( player, SFX_TITAN_SWORD_LAUNCH_1P )
	}


	wait 0.35 




}















































void function TitanSword_Launcher_TryLauncherAnimEvent( entity weapon )
{

		if ( !InPrediction() || !IsFirstTimePredicted() )
			return


	if ( !weapon.HasMod( TITAN_SWORD_LAUNCHER_MOD ) )
		return

	printt( "LAUNCHING" )

	entity player = weapon.GetWeaponOwner()

	if ( !IsValid( player ) )
		return

	player.EnableSlowMo()

	float velZ        = GetWeaponInfoFileKeyField_GlobalFloat( TITAN_SWORD_WEAPON_REF, "launcher_vel_z" )
	vector currentVel = player.GetVelocity()

	player.PlayerLaunch( <currentVel.x, currentVel.y, velZ>, false )
}















































bool function TitanSword_Launcher_VictimHitOverride( entity weapon, entity attacker, entity victim, vector velocity )
{
	if ( weapon.HasMod( TITAN_SWORD_LAUNCHER_MOD ) )
	{
		if ( !IsFriendlyTeam( attacker.GetTeam(), victim.GetTeam() ) )
		{
			float velZ = GetWeaponInfoFileKeyField_GlobalFloat( TITAN_SWORD_WEAPON_REF, "launcher_vel_z" )
			TitanSword_LaunchEntity( victim, <0, 0, velZ> )



			return true
		}
	}

	return false
}

void function TitanSword_Launcher_AirControl( entity player )
{
	if ( !IsValid( player ) )
		return

	player.kv.airSpeed        = 300
	player.kv.airAcceleration = 1000

	
	
}

                               