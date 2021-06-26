../bin/protoc-erl -I. usp_record_1_1.proto
erlc -I../include usp_record_1_1.erl
erl

rr("usp_record_1_1.hrl").
usp_record_1_1:encode_msg(#'NoSessionContextRecord'{payload=3}).
usp_record_1_1:encode_msg(#'SessionContextRecord'{session_id=1,sequence_id=2,expected_id=3,retransmit_id=4,
payload_sar_state=2,payloadrec_sar_state=2,payload=[0]}).

usp_record_1_1:encode_msg(#'Record'{version="abc",to_id="cd",from_id="as",payload_security=0,mac_signature=2,sender_cert=2,
record_type={no_session_context=2,#'NoSessionContextRecord'{payload=3}}}).

-record('Record',
        {version                :: iodata() | undefined, % = 1
         to_id                  :: iodata() | undefined, % = 2
         from_id                :: iodata() | undefined, % = 3
         payload_security       :: 'PLAINTEXT' | 'TLS12' | integer() | undefined, % = 4, enum Record.PayloadSecurity
         mac_signature          :: integer() | undefined, % = 5, 32 bits
         sender_cert            :: integer() | undefined, % = 6, 32 bits
         record_type            :: {no_session_context, usp_record_1_1:'NoSessionContextRecord'()} | {session_context, usp_record_1_1:'SessionContextRecord'()} | undefined % oneof
        }).
		
Bin = v(-1).
usp_record_1_1:decode_msg(Bin, 'SessionContextRecord').


---------------------------------------------------------------------------
../bin/protoc-erl -I. test.proto

erlc -I../include test.erl

erl

rr("test.hrl").
test:encode_msg(#'Person'{name="abc def", id=345, email="a@example.com"}).

test:encode_msg(#'Cat'{name="Tom", age=345}).
 
Bin = v(-1).
 
test:decode_msg(Bin, 'Person').