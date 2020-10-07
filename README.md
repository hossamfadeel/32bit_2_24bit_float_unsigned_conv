# 32bit_2_24bit_float_unsigned_conv

I was looking for answer of the same issue and I tried and adapt the following method:

u32 Value1 = 1072064103; // (0x3FE66667)16 || (0d1072064103)10`
u32 u32Value;
float Value2; 
union {
	u8 u8val[4];
	u32 u32val;
} u8_u32_conv;


                    printf("Value1 is = 0x%x\r\n ",Value1);
					memcpy (&Value2, &Value1, 4);
					memcpy (&u32Value, &Value2, 4);

					u32Value = ~(u32Value) + 1;
					printf("u32Value after 2's complement= = 0x%x\r\n ", u32Value);
					printf("u32Value after 2's complement= = %ld\r\n ", u32Value);
					printf("==============================\r\n ");

					u8_u32_conv.u32val = u32Value;
					u8Value[0] = u8_u32_conv.u8val[1];
					u8Value[1] = u8_u32_conv.u8val[2];
					u8Value[2] = ((u8_u32_conv.u8val[3] & 0x80) ? (0x80 | u8_u32_conv.u8val[3]) : (0x7F & u8_u32_conv.u8val[3]));
					printf("u8Value = 0x%x%x%x\r\n ", u8Value[2],u8Value[1],u8Value[0]);

					//Reverse for check
					u8_u32_conv.u8val[0] = u8Value[0]; // low byte
					u8_u32_conv.u8val[1] = u8Value[1]; // middle byte
					u8_u32_conv.u8val[2] = u8Value[2]; // high byte
					u8_u32_conv.u8val[3] = (u8Value[2] & 0x80 ? 0xFF : 0);

					u8_u32_conv.u32val = u8_u32_conv.u32val - 1;
					u8_u32_conv.u32val = ~u8_u32_conv.u32val;
					u8_u32_conv.u32val = u8_u32_conv.u32val << 8;
					xil_printf("Original Value = 0x%x\r\n ", u8_u32_conv.u32val);

					memcpy (&ReverseVal, &u8_u32_conv.u32val, 4); //Convert to float

					printf("ReverseVal = %f\r\n ", ReverseVal);``

