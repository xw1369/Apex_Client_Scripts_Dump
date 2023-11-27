

global function MeleeFuseHeirloom_Init

global function OnWeaponActivate_melee_fuse_heirloom
global function OnWeaponDeactivate_melee_fuse_heirloom


const FUSE_GUITAR_FX_SWIPE_FP = $"P_fuse_guitar_base_swipe"
const FUSE_GUITAR_FX_SWIPE_3P = $"P_fuse_guitar_base_swipe_3P"
const FUSE_GUITAR_FX_SWIPE_THURSTERS_FP = $"P_fuse_guitar_thurster_swipe"
const FUSE_GUITAR_FX_SWIPE_THRUSTERS_3P = $"P_fuse_guitar_thurster_swipe_3P"
const FUSE_GUITAR_FX_SWIPE_VENT_FP = $"P_fuse_guitar_vent_swipe"
const FUSE_GUITAR_FX_SWIPE_VENT_3P = $"P_fuse_guitar_vent_swipe_3P"
const FUSE_GUITAR_FX_SWIPE_BASE_FP = $"P_fuse_guitar_base" 
const FUSE_GUITAR_FX_SWIPE_BASE_3P = $"P_fuse_guitar_base_3P"





void function MeleeFuseHeirloom_Init()
{
	
	PrecacheParticleSystem( FUSE_GUITAR_FX_SWIPE_FP )
	PrecacheParticleSystem( FUSE_GUITAR_FX_SWIPE_3P )
	PrecacheParticleSystem( FUSE_GUITAR_FX_SWIPE_THURSTERS_FP )
	PrecacheParticleSystem( FUSE_GUITAR_FX_SWIPE_THRUSTERS_3P )
	PrecacheParticleSystem( FUSE_GUITAR_FX_SWIPE_VENT_FP )
	PrecacheParticleSystem( FUSE_GUITAR_FX_SWIPE_VENT_3P )
	PrecacheParticleSystem( FUSE_GUITAR_FX_SWIPE_BASE_FP )
	PrecacheParticleSystem( FUSE_GUITAR_FX_SWIPE_BASE_3P )





}

void function OnWeaponActivate_melee_fuse_heirloom( entity weapon )
{
	entity player = weapon.GetWeaponOwner()
	string meleeSkinName = MeleeSkin_GetSkinNameFromPlayer( player )

	if ( meleeSkinName == "heirloom" )
	{
		
		if ( player.IsCrouched() || player.IsSliding() )
		return
		weapon.PlayWeaponEffect( FUSE_GUITAR_FX_SWIPE_FP, FUSE_GUITAR_FX_SWIPE_3P, "fx_base_smear" )
		weapon.PlayWeaponEffect( FUSE_GUITAR_FX_SWIPE_THURSTERS_FP, FUSE_GUITAR_FX_SWIPE_THRUSTERS_3P, "fx_spike_01" )
		weapon.PlayWeaponEffect( FUSE_GUITAR_FX_SWIPE_THURSTERS_FP, FUSE_GUITAR_FX_SWIPE_THRUSTERS_3P, "fx_spike_02" )
		weapon.PlayWeaponEffect( FUSE_GUITAR_FX_SWIPE_THURSTERS_FP, FUSE_GUITAR_FX_SWIPE_THRUSTERS_3P, "fx_spike_03" )
		weapon.PlayWeaponEffect( FUSE_GUITAR_FX_SWIPE_THURSTERS_FP, FUSE_GUITAR_FX_SWIPE_THRUSTERS_3P, "fx_spike_04" )
		weapon.PlayWeaponEffect( FUSE_GUITAR_FX_SWIPE_VENT_FP, FUSE_GUITAR_FX_SWIPE_VENT_3P, "fx_vent_btm_back" )
		weapon.PlayWeaponEffect( FUSE_GUITAR_FX_SWIPE_VENT_FP, FUSE_GUITAR_FX_SWIPE_VENT_3P, "fx_vent_top_back" )
		weapon.PlayWeaponEffect( FUSE_GUITAR_FX_SWIPE_VENT_FP, FUSE_GUITAR_FX_SWIPE_VENT_3P, "fx_vent_btm_front" )
		weapon.PlayWeaponEffect( FUSE_GUITAR_FX_SWIPE_VENT_FP, FUSE_GUITAR_FX_SWIPE_VENT_3P, "fx_vent_top_front" )

	}









}

void function OnWeaponDeactivate_melee_fuse_heirloom( entity weapon )
{
	entity player = weapon.GetWeaponOwner()
	string meleeSkinName = MeleeSkin_GetSkinNameFromPlayer( player )

	if ( meleeSkinName == "heirloom" )
	{
		
		weapon.StopWeaponEffect( FUSE_GUITAR_FX_SWIPE_FP, FUSE_GUITAR_FX_SWIPE_3P )
		weapon.StopWeaponEffect( FUSE_GUITAR_FX_SWIPE_THURSTERS_FP, FUSE_GUITAR_FX_SWIPE_THRUSTERS_3P )
		weapon.StopWeaponEffect( FUSE_GUITAR_FX_SWIPE_VENT_FP, FUSE_GUITAR_FX_SWIPE_VENT_3P )
	}







}

                              