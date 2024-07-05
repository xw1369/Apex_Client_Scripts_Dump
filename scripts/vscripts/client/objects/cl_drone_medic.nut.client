global function ClDroneMedic_Init


void function ClDroneMedic_Init()
{
	DroneFX_Init( $"mdl/props/lifeline_drone/lifeline_drone.rmdl" )
	DroneFX_Init( $"mdl/props/lifeline_drone/lifeline_mythic_drone.rmdl" )
}

void function DroneFX_Init( asset model )
{
	ModelFX_BeginData( "thrusters", model, "all", false )

	
	
	
	ModelFX_AddTagSpawnFX( "vent_LF", $"P_LL_med_drone_jet_loop" )
	ModelFX_AddTagSpawnFX( "vent_LR", $"P_LL_med_drone_jet_loop" )
	ModelFX_AddTagSpawnFX( "vent_RR", $"P_LL_med_drone_jet_loop" )
	ModelFX_AddTagSpawnFX( "vent_RF", $"P_LL_med_drone_jet_loop" )
	ModelFX_AddTagSpawnFX( "vent_bot", $"P_LL_med_drone_jet_ctr_loop" )

	ModelFX_EndData()
	
	
	ModelFX_BeginData( "thrustersleftside_burst",model, "all", false )

	
	ModelFX_AddTagSpawnFX( "vent_LF", $"P_emote_lifeline_dj_drone_jet" )
	ModelFX_AddTagSpawnFX( "vent_LR", $"P_emote_lifeline_dj_drone_jet" )

	ModelFX_EndData()
	
	
	ModelFX_BeginData( "thrustersrightside_burst", model, "all", false )

	
	ModelFX_AddTagSpawnFX( "vent_RF", $"P_emote_lifeline_dj_drone_jet" )
	ModelFX_AddTagSpawnFX( "vent_RR", $"P_emote_lifeline_dj_drone_jet" )

	ModelFX_EndData()
	
	
	ModelFX_BeginData( "eyeglow_burst", model, "all", false )

	
	ModelFX_AddTagSpawnFX( "EYEGLOW", $"P_emote_lifeline_dj_drone_eye" )

	ModelFX_EndData()
	

	ModelFX_BeginData( "eyeglow", model, "all", false )
	
	
	
	ModelFX_AddTagSpawnFX( "EYEGLOW", $"P_LL_med_drone_eye"  )

	ModelFX_EndData()

	ModelFX_BeginData( "thrusters_attack", model, "all", false )

	
	
	
	ModelFX_AddTagSpawnFX( "vent_bot", $"P_LL_med_drone_jet_ctr_loop_attk" )

	ModelFX_EndData()

	ModelFX_BeginData( "eyeglow_attack", model, "all", false )

	
	
	
	ModelFX_AddTagSpawnFX( "EYEGLOW", $"P_lifeline_mythic_exec_eyes_attack"  )

	ModelFX_EndData()

	ModelFX_BeginData( "thrusters_jet_attack", model, "all", false )
	
	
	
	ModelFX_AddTagSpawnFX( "vent_LF", $"P_lifeline_mythic_exec_jet_loop_attk" )
	ModelFX_AddTagSpawnFX( "vent_LR", $"P_lifeline_mythic_exec_jet_loop_attk" )
	ModelFX_AddTagSpawnFX( "vent_RR", $"P_lifeline_mythic_exec_jet_loop_attk" )
	ModelFX_AddTagSpawnFX( "vent_RF", $"P_lifeline_mythic_exec_jet_loop_attk" )
	ModelFX_AddTagSpawnFX( "vent_bot", $"P_LL_med_drone_jet_ctr_loop_attk" )

	ModelFX_EndData()


}



