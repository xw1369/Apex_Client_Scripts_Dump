global function ClKillReplayFX_Init

struct {
	var killreplayVictimTagRUI
} file

void function ClKillReplayFX_Init()
{
	AddCallback_KillReplayStarted( ClKillReplayFX_ReplayBegin )
	AddCallback_KillReplayEnded( ClKillReplayFX_ReplayEnd )
}

void function ClKillReplayFX_ReplayBegin()
{
	Assert( file.killreplayVictimTagRUI == null )
	file.killreplayVictimTagRUI = ReconScan_ShowHudForTarget( GetLocalViewPlayer(), GetLocalClientPlayer().GetKillReplayVictim() )
}

void function ClKillReplayFX_ReplayEnd()
{
	RuiDestroy( file.killreplayVictimTagRUI )
}
