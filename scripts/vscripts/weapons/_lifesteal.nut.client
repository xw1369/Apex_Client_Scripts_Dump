global function Lifesteal_Init


global function ServerToClient_ShowLifesteal







const string LIFESTEAL_WEAPON_VAR = "lifesteal_heal_percent"


const float VALENTINES_HEAL_DEBOUNCE_SEC = 10


 const VFX_COCKPIT_HEALTH = $"P_heal_loop_screen"
 const VFX_COCKPIT_SHIELDS = $"P_armor_FP_charging_CP"
 const VFX_PLAYER_HEALED_3P = $"P_armor_3P_loop_CP"







const SFX_RECEIVING_HEAL_1P = "DateNight_AOE_Bow_Healing_Success_1P"
const SFX_GAVE_HEAL_1P = "DateNight_AOE_Success_Stinger_1P"
const SFX_RECEIVING_HEAL_3P = "DateNight_AOE_Success_Stinger_3P"

struct
{

		bool  isHealing = false
		float healedEndTime
		int healAmountTotal
		var healRui

} file

void function Lifesteal_Init()
{
	PrecacheParticleSystem( VFX_COCKPIT_HEALTH )
	PrecacheParticleSystem( VFX_COCKPIT_SHIELDS )
	PrecacheParticleSystem( VFX_PLAYER_HEALED_3P )

	Remote_RegisterClientFunction( "ServerToClient_ShowLifesteal", "bool", "int", INT_MIN, INT_MAX )


	AddCallback_GameStateEnter( eGameState.Postmatch, Lifesteal_OnGameState_Ending )

}
































































































































































const float HEAL_VFX_DURATION = 1
const float HEAL_HUD_LINGER = 1.5
void function ServerToClient_ShowLifesteal( bool onlyShields, int amount )
{
	if ( Time() > file.healedEndTime )
		file.healAmountTotal = amount
	else
		file.healAmountTotal += amount

	file.healedEndTime = (Time() + HEAL_VFX_DURATION )
	entity player = GetLocalViewPlayer()
	int armorTier = EquipmentSlot_GetEquipmentTier( player, "armor" )
	armorTier = armorTier <= 0 ? 1 : armorTier

	if ( !file.isHealing )
		thread HealVFX_Thread( onlyShields, armorTier )

	ShowHealHUD( file.healedEndTime + HEAL_HUD_LINGER, onlyShields, file.healAmountTotal, armorTier )
}

void function ShowHealHUD( float endTime, bool onlyShields, int amount, int armorTier )
{
	if ( file.healRui == null )
		file.healRui = CreateCockpitPostFXRui( $"ui/lifesteal_hud.rpak" , MINIMAP_Z_BASE )

	RuiSetGameTime( file.healRui, "endTime", endTime )
	RuiSetBool( file.healRui, "onlyShields", onlyShields )
	RuiSetInt( file.healRui, "healAmount", amount )
	RuiSetInt( file.healRui, "shieldTier", armorTier )
}

void function HealVFX_Thread( bool onlyShields, int armorTier )
{
	entity player = GetLocalViewPlayer()

	player.EndSignal( "OnDeath" )
	player.EndSignal( "OnDestroy" )

	file.isHealing = true

	int fxID = onlyShields ? GetParticleSystemIndex( VFX_COCKPIT_SHIELDS ) : GetParticleSystemIndex( VFX_COCKPIT_HEALTH )

	int fxHandle = StartParticleEffectOnEntity( player, fxID, FX_PATTACH_ABSORIGIN_FOLLOW, ATTACHMENTID_INVALID )
	EffectSetIsWithCockpit( fxHandle, true )
	if ( onlyShields )
	{
		vector shieldColor = GetFXRarityColorForTier( armorTier )
		EffectSetControlPointVector( fxHandle, 1, shieldColor )
	}
	OnThreadEnd(
		function() : (fxHandle)
		{
			file.isHealing = false
			if ( EffectDoesExist( fxHandle ) )
				EffectStop( fxHandle, false, true )
		}
	)

	while( (file.healedEndTime > Time()) )
		WaitFrame()
}

void function Lifesteal_OnGameState_Ending()
{
	if ( file.healRui == null )
		return

	RuiDestroyIfAlive( file.healRui )
	file.healRui = null
}

