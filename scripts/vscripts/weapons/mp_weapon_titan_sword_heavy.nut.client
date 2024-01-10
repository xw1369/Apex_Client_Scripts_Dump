
global function MpWeaponTitanSword_Heavy_Init
global function TitanSword_Heavy_OnWeaponActivate
global function TitanSword_Heavy_ClearMods
global function TitanSword_Heavy_TryHeavyAttack

global function TitanSword_TryMultiAttack

const string TITAN_SWORD_HEAVY_MOD = "heavy_melee"
const string TITAN_SWORD_HEAVY_SUPER_MOD = "super_melee"


const bool TITAN_SWORD_LOS_DEBUG = false
const bool DEBUG_MULTI_HIT = false


const float TITAN_SWORD_HEAVY_ATTACK_DISTANCE = 150
const float TITAN_SWORD_HEAVY_ATTACK_ANGLE_HORIZONTAL = 90
const float TITAN_SWORD_HEAVY_ATTACK_ANGLE_VERTICAL = 30

const float TITAN_SWORD_INSTRUCTIONS_DEBOUNCE_TIME = 10


const TITAN_SWORD_FX_HEAVY_ATK_FP = $"P_pilot_sword_swipe_heavy_FP"
const TITAN_SWORD_FX_HEAVY_ATK_3P = $"P_pilot_sword_swipe_heavy_3P"



struct
{
}file


void function MpWeaponTitanSword_Heavy_Init()
{
	PrecacheParticleSystem( TITAN_SWORD_FX_HEAVY_ATK_FP )
	PrecacheParticleSystem( TITAN_SWORD_FX_HEAVY_ATK_3P )
}

void function TitanSword_Heavy_StartVFX( entity weapon )
{
	weapon.PlayWeaponEffect( TITAN_SWORD_FX_HEAVY_ATK_FP, TITAN_SWORD_FX_HEAVY_ATK_3P, "blade_turn" )
}

void function TitanSword_Heavy_StopVFX( entity weapon )
{
	weapon.StopWeaponEffect( TITAN_SWORD_FX_HEAVY_ATK_FP, TITAN_SWORD_FX_HEAVY_ATK_3P )
}

void function TitanSword_Heavy_OnWeaponActivate( entity player, entity weapon )
{




}

void function TitanSword_Heavy_ClearMods( entity weapon )
{
	weapon.RemoveMod( TITAN_SWORD_HEAVY_MOD )
	weapon.RemoveMod( TITAN_SWORD_HEAVY_SUPER_MOD )

	TitanSword_Heavy_StopVFX( weapon )
}


bool function TitanSword_Heavy_TryHeavyAttack( entity player, entity weapon )
{

		if ( !InPrediction() )
			return false


	TitanSword_SafelyAddAttackMod( weapon, TITAN_SWORD_HEAVY_MOD )

	if ( TitanSword_Super_IsActive( player ) )
		weapon.AddMod( TITAN_SWORD_HEAVY_SUPER_MOD )

	TitanSword_Heavy_StartVFX( weapon )
	return true
}


bool function TitanSword_TryMultiAttack( entity attacker, entity weapon, int impactTableOverride )
{
	if ( !TitanSword_WeaponIsTitanSword( weapon ) )
		return false

	if ( !weapon.HasMod( TITAN_SWORD_HEAVY_MOD ) )
		return false

	array<entity> hitList = PlayerMelee_MultiAttackTrace( attacker, TITAN_SWORD_HEAVY_ATTACK_DISTANCE, TITAN_SWORD_HEAVY_ATTACK_ANGLE_HORIZONTAL, TITAN_SWORD_HEAVY_ATTACK_ANGLE_VERTICAL,
				(int function( entity blockingEntity ) {
			return TitanSword_CanGoThroughBlockingEntity( blockingEntity ) 
		}),
				(bool function( entity attacker, entity target ) : ( weapon ) {
			return IsValidMeleeAttackTarget( attacker, null, weapon, target ) 
		}),
				(void function( entity attacker, table attackInfo ) : ( weapon, impactTableOverride ) {
			TitanSword_OnMultiAttackHit( attacker, attackInfo, weapon, impactTableOverride ) 
		}) )

	return hitList.len() > 0
}


void function TitanSword_OnMultiAttackHit( entity attacker, table attackInfo, entity meleeAttackWeapon, int impactTableOverride )
{
	Melee_Attack( attacker, meleeAttackWeapon, impactTableOverride, attackInfo ) 
}

                               